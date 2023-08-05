---
id: e1figxo6pn9wnmeptpogic0
title: Tool Readme
desc: ''
updated: 1691248845724
created: 1691198475107
---

hezhong^ep37^
=========
***
**detail description**


### Getting started
___
#### launch

#### feature
***
##### ep37 sdk
- [x] dms alarm
- [x] dms face

##### sdk configure
- [x] init param

##### material fetch way
- [x] read local dir
- [x] read remote `http` url  
>
##### cfg customization
- [x] result folder
- [x] record folder
- [x] predefine task list
- [x] number of repeat operation for each batch material run
- [x] run time unit `millisecond` for each pic material run 

##### 


### order
1. choose one subcommand append to program tail
- local
- remote
>  \#  ./exe  local
2. append to option and flags
- -c/--cfg
- -e/--empty_sdk_call
> \# ./exe  local -c ./cfg.toml -e <bar>

> each option or flags has two name, short name use `-` , full name use `--`
e.g. `-c == --cfg` `-empty_sdk_call == -e` 

----
### complete example
1. **run local cfg task, use real sdk call**
> \# ./exe local -c ./cfg.toml
2. **run local cfg task, use empty sdk call**
> \# ./exe local -c ./cfg.toml -e