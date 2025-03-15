# ruyisdk-gnu-tests

在各开发板上验证 ruyisdk 的 gnu-plct、gnu-upstream 工具链的安装和可用性。

主要编译验证项目包括但不限于：

- Hello World
- CoreMark

## 测试情况

| GCC 编译器/开发板（SoC） | D1 (c906fdv) | SpacemiT K1/M1 (X60) | TH1520 (c910) | JH7110 (U74) | K230 (c908) | SG2042 (c920v2) | CV1800B (c906fdv) | SG2000 (c906fdv) |
|--------------------------|--------------|----------------------|---------------|--------------|-------------|-----------------|-------------------|------------------|
| gnu-upstream             | TBD          | ✅ PASS              | TBD           | TBD          | ✅ PASS     | TBD             | TBD               | TBD              |
| gnu-plct                 | TBD          | ✅ PASS              | TBD           | TBD          | ✅ PASS     | TBD             | TBD               | ✅ PASS          |

## License

TBD

## Credits

此仓库的测试报告由以下成员完成：

- D1 (c906fdv): @panglars
- SpacemiT K1/M1 (X60): @255doesnotexist
- TH1520 (c910): @wychlw
- JH7110 (U74): @saicogn
- K230 (c908): @aisuneko
- SG2042 (c920v2): 待分配
- CV1800B (c906fdv): 待分配
- SG2000 (c906fdv): @zeyi2
