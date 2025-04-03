---
id: k5ammue8yhwtyr7y0c9c2xo
title: Rkmedia_muxer
desc: ''
updated: 1743060189089
created: 1742890753786
---
### 只进行视频muxer失败，提示audio相关的错误

```bash
C:\Users\Administrator\AppData\Roaming\MobaXterm\home>adb push \\wsl.localhost\Ubuntu-22.04\home\xjf1127\codespace\git_down\new_aplus\test\takephoto\build-test_root-rk1126_arm_linux-Debug\target\o_r
kmedia_muxer_test\rkmedia_muxer_test /oem/xjf1127

adb pull /userdata/MuxerCbTest_* \\wsl.localhost\Ubuntu-22.04\home\xjf1127\codespace\git_down\new_aplus\test\takephoto\data

[root@RV1126_RV1109:/oem/xjf1127]# ./rkmedia_muxer_test -a /etc/iqfiles -e h264 -w 3840 -h 2160 -d rkispp_scale0
/bin/sh: ./rkmedia_muxer_test: Permission denied
[root@RV1126_RV1109:/oem/xjf1127]# chmod 777 *
[root@RV1126_RV1109:/oem/xjf1127]# ./rkmedia_muxer_test -a /etc/iqfiles -e h265
-w 3840 -h 2160 -d rkispp_scale0
media get entity by name: rkisp-mpfbc-subdev is null
media get entity by name: rkisp_dmapath is null
media get entity by name: rkcif_dvp is null
media get entity by name: rkcif_mipi_lvds is null
media get entity by name: rkcif_dvp is null
media get entity by name: rkcif_lite_mipi_lvds is null
media get entity by name: rkcif_mipi_lvds is null
[16:36:14.022950][CAMHW]:XCAM ERROR CamHwIsp20.cpp:1022: No free isp&ispp needed by fake camera!
register factory : rkmpp
register factory : rkmpp
register factory : live555_rtsp_server
register factory : video_enc
register factory : audio_enc
register factory : video_dec
register factory : file_read_flow
register factory : file_write_flow
register factory : filter
register factory : link_flow
register factory : source_stream
register factory : muxer_flow
register factory : audio_dec
register factory : output_stream
register factory : move_detec
register factory : occlusion_detec
register factory : file_write_stream
register factory : file_read_stream
register factory : alsa_playback_stream
register factory : alsa_capture_stream
register factory : v4l2_capture_stream
register factory : v4l2_output_stream
register factory : drm_output_stream
rga_api version 1.3.0_[11] (RGA is compiling with meson base: $PRODUCT_BASE)
register factory : rkrga
register factory : through_guard
register factory : ANR
register factory : AEC
register factory : rockx_filter
register factory : nn_result_input
register factory : draw_filter
register factory : rknn
register factory : face_capture
register factory : rockface_detect
register factory : rockface_evaluate
register factory : rockface_bodydetect
register factory : rockface_recognize
#Device: rkispp_scale0
#CodecName: H265
#Resolution: 3840x2160
#EnableCallback: -1
#CameraIdx: 0
#Aiq MultiCtx: 0
#Aiq XML Dir: /etc/iqfiles
ID: 0, sensor_name is m01_f_sc8238 1-0030, iqfiles is /etc/iqfiles
[16:36:14.495081][XCORE]:XCAM ERROR v4l2_device.cpp:196: open device() failed
[16:36:14.497909][CAMHW]:XCAM ERROR LensHw.cpp:199: get vcm cfg failed
[16:36:14.515849][XCORE]:XCAM ERROR CamHwIsp20.cpp:3751: |||set DC-Iris PwmDuty: 100
[16:36:14.515966][CAMHW]:XCAM ERROR LensHw.cpp:411: iris is not supported
[16:36:14.545047][CAMHW]:XCAM ERROR LensHw.cpp:296: focus is not supported
rk_aiq_uapi_sysctl_init/prepare succeed
rk_aiq_uapi_sysctl_start succeed
SAMPLE_COMM_ISP_SetFrameRate start 30
SAMPLE_COMM_ISP_SetFrameRate 30
##RKMEDIA Log level: 2
[RKMEDIA][SYS][Info]:text is all=2
[RKMEDIA][SYS][Info]:module is all, log_level is 2
[RKMEDIA][SYS][Info]:RK_MPI_VI_EnableChn: Enable VI[0:0]:rkispp_scale0, 3840x2160 Start...
[RKMEDIA][SYS][Info]:RKAIQ: parsing /dev/media0
media get entity by name: rkisp-mpfbc-subdev is null
media get entity by name: rkisp_dmapath is null
[RKMEDIA][SYS][Info]:RKAIQ: model(rkisp0): isp_info(0): isp-subdev entity name: /dev/v4l-subdev1
[RKMEDIA][SYS][Info]:RKAIQ: parsing /dev/media1
[RKMEDIA][SYS][Info]:RKAIQ: model(rkispp0): ispp_info(0): ispp-subdev entity name: /dev/v4l-subdev0
[RKMEDIA][SYS][Info]:#V4l2Stream: camraID:0, Device:rkispp_scale0
[RKMEDIA][SYS][Warn]:camera_id: 0, chn: rkispp_scale0
[RKMEDIA][SYS][Warn]:camera_id: 0, chn: rkispp_scale0, idx: 0
[RKMEDIA][SYS][Info]:#V4l2Stream: camera id:0, VideoNode:/dev/video14
Using mplane plugin for capture
[RKMEDIA][SYS][Info]:#V4L2Ctx: open /dev/video14, fd 91
[RKMEDIA][SYS][Info]:RK_MPI_VI_EnableChn: Enable VI[0:0]:rkispp_scale0, 3840x2160 End...
[RKMEDIA][SYS][Info]:RK_MPI_VENC_CreateChn: Enable VENC[0], Type:6 Start...
[RKMEDIA][SYS][Info]:ParseMediaConfigFromMap: rect_x = 0, rect_y = 0, rect.w = 3840, rect.h = 2160
mpp[2599]: mpp_rt: NOT found ion allocator
mpp[2599]: mpp_rt: found drm allocator
mpp[2599]: mpp_info: mpp version: 57ff4c6b author: Herman Chen   2021-09-13 [cmake]: Enable HAVE_DRM by default
[RKMEDIA][VENC][Info]:MPP Encoder: MPPConfig: cfg init sucess!
[RKMEDIA][VENC][Info]:MPP Encoder: qpMaxi use default value:48
[RKMEDIA][VENC][Info]:MPP Encoder: qpMini use default value:8
[RKMEDIA][VENC][Info]:MPP Encoder: qpMax use default value:48
[RKMEDIA][VENC][Info]:MPP Encoder: qpMin use default value:8
[RKMEDIA][VENC][Info]:MPP Encoder: qpInit use default value:-1
[RKMEDIA][VENC][Info]:MPP Encoder: qpStep use default value:2
[RKMEDIA][VENC][Info]:MPP Encoder: rect_x use default value:0
[RKMEDIA][VENC][Info]:MPP Encoder: rect_y use default value:0
[RKMEDIA][VENC][Info]:MPP Encoder: rotaion = 0
[RKMEDIA][VENC][Info]:MPP Encoder: automatically calculate bsp with bps_target
[RKMEDIA][VENC][Info]:MPP Encoder: Set output block mode.
[RKMEDIA][VENC][Info]:MPP Encoder: Set input block mode.
[RKMEDIA][VENC][Info]:MPP Encoder: bps:[9216000,8294400,7372800] fps: [30/1]->[30/1], gop:30 qpInit:-1, qpMin:8, qpMax:48, qpMinI:8, qpMaxI:48.
mpp[2599]: mpp_enc: MPP_ENC_SET_RC_CFG bps 8294400 [7372800 : 9216000] fps [30:30] gop 30
mpp[2599]: h265e_api: h265e_proc_prep_cfg MPP_ENC_SET_PREP_CFG w:h [3840:2160] stride [3840:2160]
mpp[2599]: mpp_enc: send header for set cfg change input/format
[RKMEDIA][VENC][Info]:MPP Encoder: w x h(3840[3840] x 2160[2160])
mpp[2599]: mpp_enc: mode cbr bps [7372800:8294400:9216000] fps fix [30/1] -> fix [30/1] gop i [30] v [0]
[RKMEDIA][SYS][Info]:RK_MPI_VENC_CreateChn: Enable VENC[0], Type:6 End...
#MuxerTest: use split name callback type, cb:0x12ae8, handle:0x271f0...
[RKMEDIA][SYS][Info]:Muxer will use internal path
[RKMEDIA][SYS][Info]:Muxer will use default prefix
[RKMEDIA][SYS][Info]:Muxer will save video file per 30sec
[RKMEDIA][SYS][Info]:Muxer: pre_record_mode 0
[RKMEDIA][SYS][Info]:Muxer: pre-record time 0(s)
[RKMEDIA][SYS][Info]:Muxer: pre-record cache time 0(s)
[RKMEDIA][SYS][Info]:Muxer: muxer_id 0
[RKMEDIA][SYS][Info]:Muxer: is_lapse_record 0
[RKMEDIA][SYS][Info]:Muxer:: enable_streaming is 0
[RKMEDIA][SYS][Info]:ParseMediaConfigFromMap: rect_x = 0, rect_y = 0, rect.w = 0, rect.h = 0
[RKMEDIA][SYS][Info]:Found video encode config!
[RKMEDIA][SYS][Info]:Muxer:: file_name_cb is 0x12ae8, file_name_handle is 0x271f0
### Start muxer stream...
### muxer_event_cb: Handle:0xaeb649a8, event:0xaeb547dc
@@@ muxer_event_cb: ModeID:20, ChnID:0, EventType:0, filename:, value:0
[RKMEDIA][SYS][Info]:RK_MPI_MUXER_Bind: Bind Mode[VENC]:Chn[0] to Mode[MUXER]:Chn[0]...
[RKMEDIA][SYS][Info]:RK_MPI_SYS_Bind: Bind Mode[VI]:Chn[0] to Mode[VENC]:Chn[0]...
main initial finish
[RKMEDIA][SYS][Info]:Camera 0 stream 91 is started
#GetRecordFileName: Handle:0x271f0 idx:0, fileIndex:0, ...
#GetRecordFileName: NewRecordFileName:[/userdata/MuxerCbTest_0.mp4]
rkaudio is not Integrated
[RKMEDIA][SYS][Info]:Create muxer rkaudio failed
mpp[2599]: mpp_meta: ~MppMetaService cleaning leaked metadata
mpp[2599]: mpp_buffer: ~MppBufferService cleaning misc group
mpp[2599]: mpp_buffer: ~MppBufferService cleaning leaked group
mpp[2599]: mpp_buffer: ~MppBufferService cleaning leaked buffer
mpp[2599]: mpp_drm: os_allocator_drm_alloc handle_to_fd failed ret -1
mpp[2599]: mpp_buffer: mpp_buffer_create failed to create buffer with size 8294400
mpp[2599]: mpp_enc: Assertion buffer failed at mpp_enc_check_pkt_buf:1164
mpp[2599]: mpp_buffer: mpp_buffer_get_ptr invalid NULL input from mpp_enc_check_pkt_buf
mpp[2599]: mpp_buffer: mpp_buffer_get_size invalid NULL input from mpp_enc_check_pkt_buf
mpp[2599]: mpp_enc: Assertion enc->pkt_buf failed at try_get_enc_task:1552
mpp[2599]: mpp_buffer: mpp_buffer_get_size invalid NULL input from vepu541_h265_set_patch_info
mpp[2599]: mpp_buffer: mpp_buffer_get_fd invalid NULL input from vepu54x_h265_set_hw_address
mpp[2599]: mpp_serivce: mpp_service_cmd_send ioctl MPP_IOC_CFG_V1 failed ret -1 errno 12 Cannot allocate memory
mpp[2599]: hal_h265e_v541: hal_h265e_v541_start send cmd failed 12
mpp[2599]: mpp_enc: mpp 0x2df30 mpp_enc_hal_start:1274 failed return 12
mpp[2599]: mpp_enc: mpp 0x2df30 mpp_enc_normal:1838 failed return 12
mpp[2599]: mpp_buffer: Assertion group failed at mpp_buffer_ref_dec:495
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: stream off....
libv4l2: error dequeuing buf: Invalid argument
[RKMEDIA][SYS][Info]:rkispp_scale0, ioctl(VIDIOC_DQBUF): Invalid argument
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: read thread exit sucessfully!
[RKMEDIA][SYS][Info]:#V4L2Ctx: close , fd 91
[RKMEDIA][SYS][Info]:#V4L2Stream: v4l2 ctx reset to nullptr!
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: stream reset sucessfully!
[RKMEDIA][SYS][Info]:SourceFlow:v4l2_capture_stream quit
[RKMEDIA][SYS][Info]:VideoEncoderFlow quit
[RKMEDIA][VENC][Info]:MPP Encoder: MPPConfig: cfg deinit done!
mpp[2599]: mpp_buffer: mpp_buffer_ref_dec found non-positive ref_count 0 caller hal_bufs_get_buf
mpp[2599]: mpp_buffer: mpp_buffer_ref_dec found non-positive ref_count 0 caller hal_bufs_get_buf
mpp[2599]: mpp_buffer: mpp_buffer_ref_dec found non-positive ref_count 0 caller (null)
mpp[2599]: mpp_buffer: mpp_buffer_ref_dec found non-positive ref_count 0 caller hal_bufs_get_buf
mpp[2599]: mpp_buffer: mpp_buffer_ref_dec found non-positive ref_count 0 caller hal_bufs_get_buf
mpp[2599]: mpp_buffer: mpp_buffer_ref_dec found non-positive ref_count 0 caller hal_bufs_get_buf
mpp[2599]: mpp_meta: Assertion meta->ref_count failed at put_meta:178
free(): double free detected in tcache 2
Aborted

```

### 模块 反射器和工厂类操作宏模板

目前看来这些辅助宏不仅仅提供了工厂管理以及反射机制，还有插件机制

```c++
//in reflector.h
#define REFLECTOR(PRODUCT) PRODUCT##Reflector  //split for reflector class
#define FACTORY(PRODUCT) PRODUCT##Factory //split for factory class

#define DECLARE_REFLECTOR(PRODUCT)    // used on .h header file
#define DEFINE_REFLECTOR(PRODUCT)    // used on .c src file

#define DECLARE_FACTORY(PRODUCT) //used on .h header file
#define DEFINE_FACTORY_COMMON_PARSE(PRODUCT) 
#define FACTORY_IDENTIFIER_DEFINITION(IDENTIFIER)
#define FACTORY_INSTANCE_DEFINITION(FACTORY) 
#define FACTORY_REGISTER(FACTORY, REFLECTOR, FINAL_EXPOSE_PRODUCT)   
#define DEFINE_ERR_GETSET()     
#define DECLARE_PART_FINAL_EXPOSE_PRODUCT(PRODUCT)  
#define DEFINE_PART_FINAL_EXPOSE_PRODUCT(FINAL_EXPOSE_PRODUCT, PRODUCT) 
#define DEFINE_CHILD_FACTORY(REAL_PRODUCT, IDENTIFIER, FINAL_EXPOSE_PRODUCT,   \
                             PRODUCT, EXTRA_CODE)   
```

宏生成代码预览，以flow举例

```c++
//in flow.h
/*
DECLARE_FACTORY(Flow)
// usage: REFLECTOR(Flow)::Create<T>(flowname, param)
// T must be the final class type exposed to user
DECLARE_REFLECTOR(Flow)

#define DEFINE_FLOW_FACTORY(REAL_PRODUCT, FINAL_EXPOSE_PRODUCT)                \
  DEFINE_MEDIA_CHILD_FACTORY(REAL_PRODUCT, REAL_PRODUCT::GetFlowName(),        \
                             FINAL_EXPOSE_PRODUCT, Flow)                       \
  DEFINE_MEDIA_CHILD_FACTORY_EXTRA(REAL_PRODUCT)                               \
  DEFINE_MEDIA_NEW_PRODUCT_BY(REAL_PRODUCT, FINAL_EXPOSE_PRODUCT,              \
                              GetError() < 0)
*/
class Flow; 
class FlowFactory { 
    public: 
    virtual const char *Identifier() const = 0; 
    static __attribute__((visibility("default"))) const char *Parse(const char *request); 
    virtual std::shared_ptr<Flow> NewProduct(const char *param) = 0; 
    bool AcceptRules(const char *rules) const { 
        std::map<std::string, std::string> map; 
        if (!parse_media_param_map(rules, map)) return false;
        return AcceptRules(map); 
    } 
    virtual bool AcceptRules(const std::map<std::string, std::string> &map) const = 0;
    protected: 
    FlowFactory() = default; 
    virtual ~FlowFactory() = default; 
    private: FlowFactory(const FlowFactory &) = delete; 
    FlowFactory &operator=(const FlowFactory &) = delete; 
};

class Flow; class FlowFactory;
class __attribute__((visibility("default"))) FlowReflector {
public:
    static const char* FindFirstMatchIdentifier(const char* rules);
    static bool IsMatch(const char* identifier, const char* rules);
    template <class T> static std::shared_ptr<T> Create(const char* request, const char* param = nullptr) {
        if (!IsDerived<T, Flow>::Result) {
            printf("The template class type is not derived of required type\n");
            return nullptr;
        }
        const char* identifier = FlowFactory::Parse(request);
        if (!identifier) return nullptr;
        auto it = factories.find(identifier);
        if (it != factories.end()) {
            const FlowFactory* f = it->second;
            if (!T::Compatible(f)) {
                printf("%s is not compatible with the template\n", request);
                return nullptr;
            }
            return std::static_pointer_cast<T>(const_cast<FlowFactory*>(f)->NewProduct(param));
        } printf("%s is not Integrated\n", request); return nullptr;
    }
    static void RegisterFactory(std::string identifier, const FlowFactory * factory);
    static void DumpFactories();
private: FlowReflector() = default;
        ~FlowReflector() = default;
        FlowReflector(const FlowReflector&) = delete;
        FlowReflector& operator=(const FlowReflector&) = delete;
        static std::map<std::string, const FlowFactory*> factories;
};


public: 
static bool Compatible(const FlowFactory *factory); 
static void RegisterFactory(const FlowFactory *factory); 
private: 
static std::list<const FlowFactory *> compatiable_factories;

// in flow.cc
/*
DEFINE_REFLECTOR(Flow)
DEFINE_FACTORY_COMMON_PARSE(Flow)
DEFINE_PART_FINAL_EXPOSE_PRODUCT(Flow, Flow)
*/
std::map<std::string, const FlowFactory *> FlowReflector::factories;

const char *FlowReflector::FindFirstMatchIdentifier(const char *rules) {
    for (auto &it : factories) {
        const FlowFactory *f = it.second;
        if (f->AcceptRules(rules))
            return it.first.c_str();
    }
    return nullptr;
}

bool FlowReflector::IsMatch(const char *identifier, const char *rules) {
    auto it = factories.find(identifier);
    if (it == factories.end()) {
        printf("%s is not Integrated\n", identifier);
        return false;
    }
    return it->second->AcceptRules(rules);
}

void FlowReflector::RegisterFactory(std::string identifier, const FlowFactory *factory) {
    auto it = factories.find(identifier);
    if (it == factories.end()) {
        factories[identifier] = factory;
        printf("register factory : %s\n", identifier.c_str());
    } else {
        printf("repeated identifier : %s\n", identifier.c_str());
    }
}

void FlowReflector::DumpFactories() {
    printf("\n%s:\n", "Flow");
    for (auto &it : factories) {
        printf(" %s", it.first.c_str());
    }
    printf("\n\n");
}

const char *FlowFactory::Parse(const char *request) { return request; }

std::list<const FlowFactory *> Flow::compatiable_factories;

bool Flow::Compatible(const FlowFactory *factory) {
    auto it = std::find(compatiable_factories.begin(), compatiable_factories.end(), factory);
    return it != compatiable_factories.end();
}

void Flow::RegisterFactory(const FlowFactory *factory) {
    auto it = std::find(compatiable_factories.begin(), compatiable_factories.end(), factory);
    if (it == compatiable_factories.end()) {
        compatiable_factories.push_back(factory);
    }
}
//in rkmedia_api.cc
flow_param = easymedia::JoinFlowParam(flow_param, 1, enc_param);
RKMEDIA_LOGD("#JPEG: Pre encoder flow param:\n%s\n", flow_param.c_str());
video_encoder_flow = easymedia::REFLECTOR(Flow)::Create<easymedia::Flow>(
    flow_name.c_str(), flow_param.c_str());
if (!video_encoder_flow) {
  RKMEDIA_LOGE("[%s]: Create flow %s failed\n", __func__,
                flow_name.c_str());
  return -RK_ERR_VENC_ILLEGAL_PARAM;
}



//in muxer_flow.cc
/**
DEFINE_FLOW_FACTORY(MuxerFlow, Flow)
const char *FACTORY(MuxerFlow)::ExpectedInputDataType() { return nullptr; }
const char *FACTORY(MuxerFlow)::OutPutDataType() { return ""; }
*/
class MuxerFlowFactory : public FlowFactory {
    public:
        const char* Identifier() const override {
            return MuxerFlow::GetFlowName();
        }
        std::shared_ptr<Flow> NewProduct(const char* param) override;
        static const MuxerFlowFactory& Instance() {
            static const MuxerFlowFactory object; return object;
        }
    private: MuxerFlowFactory() = default;
            ~MuxerFlowFactory() = default;
    public: virtual bool AcceptRules(const std::map<std::string, std::string>& map) const override;
          static const char* ExpectedInputDataType();
          static const char* OutPutDataType();
};
class Register_MuxerFlowFactory {
public:
    Register_MuxerFlowFactory() {
        const MuxerFlowFactory& obj = MuxerFlowFactory::Instance();
        FlowReflector::RegisterFactory(obj.Identifier(), &obj); Flow::RegisterFactory(&obj);
    }
};
Register_MuxerFlowFactory reg_MuxerFlowFactory;
bool MuxerFlowFactory::AcceptRules(const std::map<std::string, std::string>& map) const {
    static std::list<std::string> expected_data_type_list;
    static std::list<std::string> out_data_type_list;
    static const char* static_keys[] = { "input_data_type", "output_data_type", __null };
    static const decltype(ExpectedInputDataType)* static_call[] = { &ExpectedInputDataType, &OutPutDataType, __null };
    static std::list<std::string>* static_list[] = { &expected_data_type_list, &out_data_type_list, __null };
    const char** keys = static_keys; const decltype(ExpectedInputDataType)** call = static_call;
    std::list<std::string>** list = static_list; while (*keys) {
        auto it = map.find(*keys);
        if (it == map.end()) {
            if ((*call)()) return false;
        }
        else {
            const std::string& value = it->second;
            if (!value.empty() && !has_intersection(value.c_str(), (*call)(), *list)) return false;
        }
        ++keys; ++call; ++list;
    }
    return true;
}
std::shared_ptr<Flow> MuxerFlowFactory::NewProduct(const char* param) {
    auto ret = std::make_shared<MuxerFlow>(param);
    if (ret && ret->GetError() < 0)
        return nullptr;
    return ret;
}

const char *MuxerFlowFactory::ExpectedInputDataType() { return nullptr; }
const char *MuxerFlowFactory::OutPutDataType() { return ""; }

```

**muxer_flow相关reflector**

```c++
// in rkmedia_api.cc
//1
auto MuxerFlow = easymedia::REFLECTOR(Flow)::Create<easymedia::Flow>(
      "muxer_flow", FlowParamStr.c_str());
//2
std::string flow_name = "video_enc";
flow_param = easymedia::JoinFlowParam(flow_param, 1, enc_param);
RKMEDIA_LOGD("#JPEG: Pre encoder flow param:\n%s\n", flow_param.c_str());
video_encoder_flow = easymedia::REFLECTOR(Flow)::Create<easymedia::Flow>(
    flow_name.c_str(), flow_param.c_str());
if (!video_encoder_flow) {
    RKMEDIA_LOGE("[%s]: Create flow %s failed\n", __func__,
                flow_name.c_str());
    return -RK_ERR_VENC_ILLEGAL_PARAM;
}
// in flow.h
DECLARE_FACTORY(Flow)
// usage: REFLECTOR(Flow)::Create<T>(flowname, param)
// T must be the final class type exposed to user
DECLARE_REFLECTOR(Flow)

#define DEFINE_FLOW_FACTORY(REAL_PRODUCT, FINAL_EXPOSE_PRODUCT)                \
  DEFINE_MEDIA_CHILD_FACTORY(REAL_PRODUCT, REAL_PRODUCT::GetFlowName(),        \
                             FINAL_EXPOSE_PRODUCT, Flow)                       \
  DEFINE_MEDIA_CHILD_FACTORY_EXTRA(REAL_PRODUCT)                               \
  DEFINE_MEDIA_NEW_PRODUCT_BY(REAL_PRODUCT, FINAL_EXPOSE_PRODUCT,              \
                              GetError() < 0)

class _API Flow {
public:
private:
    DEFINE_ERR_GETSET();
    DECLARE_PART_FINAL_EXPOSE_PRODUCT(Flow);
};

//in flow.cc
DEFINE_REFLECTOR(Flow)
DEFINE_FACTORY_COMMON_PARSE(Flow)
DEFINE_PART_FINAL_EXPOSE_PRODUCT(Flow, Flow)

// in move_detection_flow.cc | occlusion_detection_flow.cc | uvc_flow.cc
if (!REFLECTOR(Flow)::IsMatch("move_detec", rule.c_str())) {
    RKMEDIA_LOGE("Unsupport for move_detec : [%s]\n", rule.c_str());
    SetError(-EINVAL);
    return;
}
if (!REFLECTOR(Flow)::IsMatch("occlusion_detec", rule.c_str())) {
    RKMEDIA_LOGE("Unsupport for occlusion_detec : [%s]\n", rule.c_str());
    SetError(-EINVAL);
    return;
}
if (!REFLECTOR(Flow)::IsMatch("uvc_flow", rule.c_str())) {
    RKMEDIA_LOGE("Unsupport for uvc_flow : [%s]\n", rule.c_str());
    SetError(-EINVAL);
    return;
}

```




**muxer相关reflector**

```c++
//in muxer.cc
namespace easymedia {

DEFINE_REFLECTOR(Muxer)

// request should equal muxer_name
DEFINE_FACTORY_COMMON_PARSE(Muxer)

Muxer::Muxer(const char *param _UNUSED)
    : io_output(nullptr), m_handler(nullptr), m_write_callback_func(nullptr) {}

DEFINE_PART_FINAL_EXPOSE_PRODUCT(Muxer, Muxer)

} // namespace easymedia

//in muxer.h
DECLARE_FACTORY(Muxer)

// usage: REFLECTOR(Muxer)::Create<T>(demuxname, param)
// T must be the final class type exposed to user
DECLARE_REFLECTOR(Muxer)

#define DEFINE_MUXER_FACTORY(REAL_PRODUCT, FINAL_EXPOSE_PRODUCT)               \
  DEFINE_MEDIA_CHILD_FACTORY(REAL_PRODUCT, REAL_PRODUCT::GetMuxName(),         \
                             FINAL_EXPOSE_PRODUCT, Muxer)                      \
  DEFINE_MEDIA_CHILD_FACTORY_EXTRA(REAL_PRODUCT)                               \
  DEFINE_MEDIA_NEW_PRODUCT_BY(REAL_PRODUCT, Muxer, Init() != true)

#define DEFINE_COMMON_MUXER_FACTORY(REAL_PRODUCT)                              \
  DEFINE_MUXER_FACTORY(REAL_PRODUCT, Muxer)

class _API Muxer {
public:
    //.....

protected:
  std::shared_ptr<Stream> io_output;
  void *m_handler;
  write_callback_func m_write_callback_func;
  DECLARE_PART_FINAL_EXPOSE_PRODUCT(Muxer)

};

//in muxer_flow.cc
VideoRecorder::VideoRecorder(const char *param, Flow *f, const char *rpath,
                             int customio)
    : vid_stream_id(-1), aud_stream_id(-1), muxer_flow(f), record_path(rpath) {
  muxer =
      easymedia::REFLECTOR(Muxer)::Create<easymedia::Muxer>("rkaudio", param);
  if (!muxer) {
    RKMEDIA_LOGI("Create muxer rkaudio failed\n");
    exit(EXIT_FAILURE);
  }
  //.....
}


```

### 编译加载 rkaudio 相关的lib后成功激活muxer功能

1. 验证获取缓冲回调的注册是否会对封装视频流的后续通道造成影响
2. 验证封装实时性，当封装参数较极端时，是否会出现封装延迟，即表现为文件io延迟，或者帧数降低，或者卡死等
3. 实验封装过程中，实时判断磁盘空间，进行录制控制，如暂停录制
