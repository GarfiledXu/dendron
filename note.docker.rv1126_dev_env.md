---
id: dke7dgr865r9bpt56ldukng
title: Rv1126_dev_en
desc: ''
updated: 1745559896038
created: 1743561434081
---

### image build

1. base ubuntu image: non-dev-conatiner,gcc,qt,cmake
2. dev-container configure:
   1. vscode plugin
   2. dotfile git repository sync
   3. vscode setting
   4. output version:
      1. inherint dev-container configure image
      2. independent dev-container configure file
3. chip-platform-sdk configured image inherient from step 2
