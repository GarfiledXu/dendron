---
id: 1jy92i7s49uzwrqvog6nus5
title: License Logic
desc: ''
updated: 1692087688013
created: 1692066490622
---

### sdk license logic
--------------
#### total flow
- check **target dir**
  - [x] true: 
    - check **file** 
      -  [x] true:
        - check **file content**
          -  [x] true: ==pass, to return success==
          -  [ ] false: 
            - offline: `return err`
            - online: ==to fetch license, return success==
      -  [ ] false:
        - offline:`return err`
        - online: ==to fetch license, return success==
  -  [ ] false:
    - offline:`return err`
    - online:`return err`


#### running scene: offline

#### running scene: online


#### prioriy scene: online success
- check **target dir**
  - [x] true: 
    - check **file** 
      -  [x] true:
        - check **file content**i
          -  [x] true: ==pass, to return success==
          -  [ ] false: 
            - online: ==to fetch license, return success==
      -  [ ] false:
        - online: ==to fetch license, return success==
  -  [ ] false:
    - online:`return err`

#### priority scenne: offline 
- check **target dir**
  - [x] true: 
    - check **file** 
      -  [x] true:
        - check **file content**i
          -  [x] true: ==pass, to return success==
          -  [ ] false: 
            - offline: `return err`
      -  [ ] false:
        - offline:`return err`
  -  [ ] false:
    - offline:`return err`