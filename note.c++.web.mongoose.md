---
id: 11ik08wpa1qg5ikrd0t1vm3
title: Mongoose
desc: ''
updated: 1709952104550
created: 1699858217004
---
#### api overview
- filesystem
- gen random, byte order convert, Calculate the CRC32 checksum, Get the current millisecond-level timestamp, help operation macro of list 
- url check and extract field (port, host name, user name and user password, uri), buff io, base64(encode, decode, update)
- MD5 和 SHA-1 
- mg_str and printf api 
- mg_log
- mg_timer
- mg_connection
- mg_dns
- mg_addrs
- mg_mgr
  - mg_mgr_poll
  - mg_mgr_init
  - mg_mgr_free
  - mg_listen
  - mg_connect
  - mg_wrapfd
  - mg_send
  - mg_printf
  - 。。。。
- mg_http
- mg_mqtt
- mg_dns
- mg_json
- mg_rpc
- mg_mip(stm32)

#### http message mapping, and operation
**struct to http request**
```c++
struct mg_str {
  const char *ptr;  // Pointer to string data
  size_t len;       // String len
};
struct mg_http_header {
  struct mg_str name;   // Header name
  struct mg_str value;  // Header value
};
struct mg_http_message {
  struct mg_str method, uri, query, proto;             // Request/response line
  struct mg_http_header headers[MG_MAX_HTTP_HEADERS];  // Headers
  struct mg_str body;                                  // Body
  struct mg_str head;                                  // Request + headers
  struct mg_str chunk;    // Chunk for chunked encoding,  or partial body
  struct mg_str message;  // Request + headers + body
};

```
**struct to http response**
```c++
// by using mg_http_reply to generate response message return directly
mg_http_reply();
```
**operation** 
```c++
int mg_http_parse(const char *s, size_t len, struct mg_http_message *hm) ;
static void mg_http_parse_headers(const char *s, const char *end,
                                  struct mg_http_header *h, int max_headers);
truct mg_str *mg_http_get_header(struct mg_http_message *h, const char *name);
int mg_url_decode(const char *src, size_t src_len, char *dst, size_t dst_len,
                  int is_form_url_encoded);
size_t mg_url_encode(const char *s, size_t n, char *buf, size_t len);
bool mg_http_match_uri(const struct mg_http_message *hm, const char *glob);                  
long mg_http_upload(struct mg_connection *c, struct mg_http_message *hm,
                    struct mg_fs *fs, const char *path, size_t max_size);
void mg_http_reply(struct mg_connection *c, int code, const char *headers,
                   const char *fmt, ...) ;
```
#### work api call flow
0. signal setting
1. init mg
2. set address + cb(parse request and reply)
3. poll
4. uninit mg

#### event drive
1. MG_EV_READ 属于socket事件
2. MG_EV_HTTP_MSG 属于protocol事件
3. protocol 事件 与 socket事件 对应不同的handler, 每次mg_call时都分别处理
```c++
void mg_call(struct mg_connection *c, int ev, void *ev_data) {
  // Run user-defined handler first, in order to give it an ability
  // to intercept processing (e.g. clean input buffer) before the
  // protocol handler kicks in
  if (c->fn != NULL) 
    c->fn(c, ev, ev_data, c->fn_data);
  if (c->pfn != NULL) 
    c->pfn(c, ev, ev_data, c->pfn_data);
}
```
4. socket 事件的handler如何影响protocol事件的处理, 如下代码只会进入read事件直到结束，不会进入httpmsg事件，为何？
```c++
static void cb(struct mg_connection *c, int ev, void *ev_data, void *fn_data) {
  if (ev == MG_EV_READ) {
    // Parse the incoming data ourselves. If we can parse the request,
    // store two size_t variables in the c->data: expected len and recv len.
    size_t *data = (size_t *) c->data;
    printf("===> enter mg read\n");
    if (data[0]) {  // Already parsed, simply print received data
      data[1] += c->recv.len;
      MG_INFO(("Got chunk len %lu, %lu total", c->recv.len, data[1]));
      c->recv.len = 0;  // And cleanup the receive buffer. Streaming!
    //   if (data[1] >= data[0]) mg_http_reply(c, 200, "", "ok\n");
    } else {
        printf("===> enter mg read else\n");
      struct mg_http_message hm;
      int n = mg_http_parse((char *) c->recv.buf, c->recv.len, &hm);
      if (n < 0) mg_error(c, "Bad response");
      if (n > 0) {
          static int test_count = 0;
        if (mg_http_match_uri(&hm, "/upload")) {
          MG_INFO(("Got chunk len %lu", c->recv.len - n));
          data[0] = hm.body.len;
          data[1] = c->recv.len - n;
        printf("===> parse success! %d, data0len:%d, data1len:%d\n", test_count++, data[0], data[1]);
        //   if (data[1] >= data[0]) mg_http_reply(c, 200, "", "ok\n");
        } else {
          struct mg_http_serve_opts opts = {.root_dir = "."};
          mg_http_serve_dir(c, &hm, &opts);
        }
      }
    }
  }
 else if (ev == MG_EV_HTTP_MSG) {
    size_t *data = (size_t *) c->data;
    printf("===> enter recive http msg!\n");
    printf("===> recive success! data0len:%d, data1len:%d\n", data[0], data[1]);
  }
  (void) fn_data, (void) ev_data;
}
```
```c++
//static http cb 
static void http_cb(struct mg_connection* c, int ev, void* evd, void* fnd) {
  if (ev == MG_EV_READ || ev == MG_EV_CLOSE) {
    struct mg_http_message hm;
    // mg_hexdump(c->recv.buf, c->recv.len);
    while (c->recv.buf != NULL && c->recv.len > 0) {
      bool next = false;
      int hlen = mg_http_parse((char *) c->recv.buf, c->recv.len, &hm);
      .....
      }}}
```
> reason: 手动处理 message 时，清空 connect 长度，以至于进入http_cb时跳出处理


#### the parse core of http
1. 按字符串规则进行 固定字段的解析
2. Content-Length or Transfer-Encoding () for body
> 由生成消息的一方，统计好 body 长度，将长度信息和 body 一起放入 request 和 response 中
如果该 header 缺失，则采用策略可能为 不断读取 buff 直到 socket close

#### my oop implementation
1. inner dependency outside declare and register to focus object
1. extract obj
2. to combine each obj and dependency injection. server(vector<connection(vector<router(vector<handler>)>)>())
3. shallow interface abstraction for adapting pure c to c++ oop
server -> run()(listen) add_connect() strip_connection() 
connection -> add_router() set_timeout()
router -> add_endpoint() add_common_filter() add_filter()
endpoint -> add_handler()
parser(friend) -> encode() decode() dto() get() set()


#### refe
[offical: mongoose](https://mongoose.ws/)
[github: mongoose](https://github.com/cesanta/mongoose)
[offical: documentation](https://mongoose.ws/documentation/)
[blog: mongoose](https://www.cnblogs.com/skynet/category/254728.html)
[blog: 核心处理模块](https://www.cnblogs.com/skynet/archive/2010/07/25/1784710.html)
[blog: mongoose7 新特性介绍以及poll过程](https://juejin.cn/post/6946962819908632606)
[blog: mongoose 初始化](https://juejin.cn/post/6905616621562724359)
[blog: Mongoose源码剖析：mongoose的工作模型](https://www.cnblogs.com/skynet/archive/2010/07/24/1784476.html)
> 已过时，该工作模型描述mongoose v6之前的版本




#### mongoose websocket 问题
**创建客户端链接后，一个线程用于poll等待服务端消息，另一个线程用于异步发送ws消息**
1. 问题1: mg_ws_send 并不会触发poll，对链接进行处理，导致每次发送的消息都是等待到poll超时后才发送到服务端
2. 按官方例程，发送线程使用 wakeup，在fn中添加wakeup事件的处理，在事件处理时调用mg_ws_send,但存在问题: 现象: 客户端消息没有发送出去就close了，服务端报错请求解析错误
3. 方案3: 发送线程正常发送，发送后调用 wakeup 只是触发poll
  