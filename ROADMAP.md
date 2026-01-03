# Solidity-Zig 路线图

> 使用 Zig 重新实现 Solidity 编译器

**目标**: 创建一个完全兼容的 Solidity 编译器，使用 Zig 语言实现，专注于代码清晰度、性能和内存安全。

**参考**: [argotorg/solidity](https://github.com/argotorg/solidity) (官方 Solidity 编译器)

---

## 项目概览

### 编译管道 (Pipeline)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          Solidity-Zig 编译管道                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  Solidity 源码                                                               │
│       │                                                                      │
│       ▼                                                                      │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐   │
│  │  Lexer  │ -> │ Parser  │ -> │   AST   │ -> │  Sema   │ -> │ Yul IR  │   │
│  │ v0.1.0  │    │ v0.2.0  │    │ v0.3.0  │    │ v0.4.0  │    │ v0.5.0  │   │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘    └─────────┘   │
│                                                                     │        │
│                                                                     ▼        │
│                                               ┌─────────┐    ┌─────────┐   │
│                                               │ EVM Gen │ <- │Optimizer│   │
│                                               │ v0.7.0  │    │ v0.6.0  │   │
│                                               └─────────┘    └─────────┘   │
│                                                      │                      │
│                                                      ▼                      │
│                                               EVM Bytecode                  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 版本里程碑

### v0.1.0 - 词法分析器 (Lexer) ⏳

**状态**: 待开始  
**预计工期**: 2-3 周  
**Story**: [stories/v0.1.0-lexer.md](stories/v0.1.0-lexer.md)

实现完整的 Solidity 词法分析器，将源代码转换为 token 流。

**核心功能**:
- [ ] Token 类型定义 (~100+ 类型)
- [ ] 关键字识别 (contract, function, mapping, uint256, 等)
- [ ] 运算符和分隔符
- [ ] 字面量 (数字、字符串、十六进制)
- [ ] 注释处理 (单行、多行、NatSpec)
- [ ] 源位置跟踪
- [ ] 错误恢复机制
- [ ] 单元测试 (>50 测试用例)

**交付物**:
- `src/lexer/tokenizer.zig`
- `src/lexer/token.zig`
- `src/lexer/source.zig`
- `docs/lexer/README.md`

---

### v0.2.0 - 语法分析器 (Parser) ⏳

**状态**: 待开始  
**依赖**: v0.1.0  
**预计工期**: 3-4 周  
**Story**: [stories/v0.2.0-parser.md](stories/v0.2.0-parser.md)

实现递归下降解析器，将 token 流转换为抽象语法树 (AST)。

**核心功能**:
- [ ] 顶层声明解析 (pragma, import, contract)
- [ ] 合约成员解析 (function, state variable, struct, enum)
- [ ] 语句解析 (if, for, while, return, 等)
- [ ] 表达式解析 (运算符优先级、函数调用)
- [ ] 类型名解析 (elementary, array, mapping)
- [ ] 修饰符和继承
- [ ] 错误恢复和多错误报告
- [ ] 单元测试 (>100 测试用例)

**交付物**:
- `src/parser/parser.zig`
- `src/parser/expressions.zig`
- `src/parser/statements.zig`
- `src/parser/declarations.zig`
- `docs/parser/README.md`

---

### v0.3.0 - 抽象语法树 (AST) ⏳

**状态**: 待开始  
**依赖**: v0.2.0  
**预计工期**: 2-3 周  
**Story**: [stories/v0.3.0-ast.md](stories/v0.3.0-ast.md)

定义完整的 AST 节点结构和访问者模式。

**核心功能**:
- [ ] AST 节点类型定义 (~60 种节点)
- [ ] 节点 ID 和源位置
- [ ] 访问者模式 (Visitor Pattern)
- [ ] AST 打印和调试输出
- [ ] JSON 序列化/反序列化
- [ ] AST 遍历工具
- [ ] 单元测试

**交付物**:
- `src/ast/nodes.zig`
- `src/ast/visitor.zig`
- `src/ast/printer.zig`
- `src/ast/json.zig`
- `docs/ast/README.md`

---

### v0.4.0 - 语义分析 (Semantic Analysis) ⏳

**状态**: 待开始  
**依赖**: v0.3.0  
**预计工期**: 4-6 周  
**Story**: [stories/v0.4.0-sema.md](stories/v0.4.0-sema.md)

实现类型系统和语义检查。

**核心功能**:
- [ ] 类型系统基础 (Type, TypeProvider)
- [ ] 基本类型 (uint, int, bool, address, bytes)
- [ ] 复合类型 (array, mapping, struct, enum)
- [ ] 函数类型和修饰符类型
- [ ] 符号表和作用域
- [ ] 名称解析 (Name Resolution)
- [ ] 类型检查 (Type Checking)
- [ ] 隐式/显式类型转换
- [ ] 继承解析
- [ ] 控制流分析
- [ ] 单元测试 (>200 测试用例)

**交付物**:
- `src/sema/types.zig`
- `src/sema/type_provider.zig`
- `src/sema/scope.zig`
- `src/sema/resolver.zig`
- `src/sema/type_checker.zig`
- `docs/sema/README.md`

---

### v0.5.0 - Yul 中间表示 (Yul IR) ⏳

**状态**: 待开始  
**依赖**: v0.4.0  
**预计工期**: 3-4 周  
**Story**: [stories/v0.5.0-yul.md](stories/v0.5.0-yul.md)

实现 Yul 中间语言及其生成器。

**核心功能**:
- [ ] Yul AST 节点定义
- [ ] Yul 解析器 (用于内联汇编)
- [ ] Solidity AST → Yul IR 转换
- [ ] 表达式 IR 生成
- [ ] 语句 IR 生成
- [ ] 函数 IR 生成
- [ ] 合约部署代码生成
- [ ] 运行时代码生成
- [ ] Yul 打印器
- [ ] 单元测试

**交付物**:
- `src/yul/ast.zig`
- `src/yul/parser.zig`
- `src/yul/ir_generator.zig`
- `src/yul/printer.zig`
- `docs/yul/README.md`

---

### v0.6.0 - Yul 优化器 ⏳

**状态**: 待开始  
**依赖**: v0.5.0  
**预计工期**: 4-5 周  
**Story**: [stories/v0.6.0-optimizer.md](stories/v0.6.0-optimizer.md)

实现 Yul IR 优化通道。

**核心功能**:
- [ ] 优化器基础架构
- [ ] 死代码消除 (Dead Code Elimination)
- [ ] 常量折叠 (Constant Folding)
- [ ] 公共子表达式消除 (CSE)
- [ ] 函数内联 (Function Inlining)
- [ ] 循环不变量外提
- [ ] 冗余赋值消除
- [ ] SSA 转换
- [ ] 优化序列配置
- [ ] 单元测试

**交付物**:
- `src/yul/optimizer/mod.zig`
- `src/yul/optimizer/passes/*.zig`
- `docs/optimizer/README.md`

---

### v0.7.0 - EVM 代码生成 ⏳

**状态**: 待开始  
**依赖**: v0.6.0  
**预计工期**: 4-5 周  
**Story**: [stories/v0.7.0-codegen.md](stories/v0.7.0-codegen.md)

实现从 Yul IR 到 EVM 字节码的转换。

**核心功能**:
- [ ] EVM 操作码定义 (151 个)
- [ ] 栈管理算法
- [ ] Yul → EVM 汇编转换
- [ ] 跳转目标解析
- [ ] 字节码组装
- [ ] Gas 计算
- [ ] 部署代码包装
- [ ] 元数据 (CBOR) 附加
- [ ] 单元测试

**交付物**:
- `src/codegen/opcodes.zig`
- `src/codegen/stack.zig`
- `src/codegen/assembler.zig`
- `src/codegen/evm_transform.zig`
- `docs/codegen/README.md`

---

### v0.8.0 - ABI 和接口 ⏳

**状态**: 待开始  
**依赖**: v0.7.0  
**预计工期**: 2-3 周  
**Story**: [stories/v0.8.0-abi.md](stories/v0.8.0-abi.md)

实现 ABI 编码/解码和编译器接口。

**核心功能**:
- [ ] ABI 类型编码
- [ ] ABI 类型解码
- [ ] 函数选择器计算
- [ ] ABI JSON 生成
- [ ] 存储布局计算
- [ ] 元数据 JSON 生成
- [ ] 命令行接口 (CLI)
- [ ] JSON 标准输入/输出
- [ ] 单元测试

**交付物**:
- `src/abi/encoder.zig`
- `src/abi/decoder.zig`
- `src/abi/json.zig`
- `src/interface/cli.zig`
- `src/interface/standard_json.zig`
- `docs/abi/README.md`

---

### v0.9.0 - 标准库和内置函数 ⏳

**状态**: 待开始  
**依赖**: v0.8.0  
**预计工期**: 3-4 周  
**Story**: [stories/v0.9.0-stdlib.md](stories/v0.9.0-stdlib.md)

实现 Solidity 内置函数和全局变量。

**核心功能**:
- [ ] 数学函数 (addmod, mulmod, 等)
- [ ] 类型函数 (type(X).min, type(X).max)
- [ ] ABI 函数 (abi.encode, abi.decode, 等)
- [ ] 密码学函数 (keccak256, sha256, 等)
- [ ] 地址成员 (balance, code, 等)
- [ ] 区块/交易全局变量
- [ ] 错误处理 (require, revert, assert)
- [ ] 预编译合约调用
- [ ] 单元测试

**交付物**:
- `src/stdlib/math.zig`
- `src/stdlib/abi_functions.zig`
- `src/stdlib/crypto.zig`
- `src/stdlib/globals.zig`
- `docs/stdlib/README.md`

---

### v1.0.0 - 完整编译器发布 ⏳

**状态**: 待开始  
**依赖**: v0.9.0  
**预计工期**: 4-6 周  
**Story**: [stories/v1.0.0-release.md](stories/v1.0.0-release.md)

完成集成、测试和发布准备。

**核心功能**:
- [ ] 端到端编译测试
- [ ] Solidity 测试套件兼容性
- [ ] 错误消息质量提升
- [ ] 文档完善
- [ ] 性能优化
- [ ] 多目标 EVM 版本支持
- [ ] 发布自动化
- [ ] 示例合约

**交付物**:
- 完整编译器二进制文件
- 全面的测试套件
- 用户文档
- API 文档

---

## 模块结构

```
src/
├── main.zig                 # CLI 入口点
├── root.zig                 # 库根模块
│
├── lexer/                   # v0.1.0 - 词法分析
│   ├── mod.zig             # 模块入口
│   ├── tokenizer.zig       # 分词器实现
│   ├── token.zig           # Token 类型定义
│   └── source.zig          # 源码位置跟踪
│
├── parser/                  # v0.2.0 - 语法分析
│   ├── mod.zig
│   ├── parser.zig          # 递归下降解析器
│   ├── declarations.zig    # 声明解析
│   ├── statements.zig      # 语句解析
│   └── expressions.zig     # 表达式解析
│
├── ast/                     # v0.3.0 - 抽象语法树
│   ├── mod.zig
│   ├── nodes.zig           # AST 节点定义
│   ├── visitor.zig         # 访问者模式
│   ├── printer.zig         # AST 打印
│   └── json.zig            # JSON 序列化
│
├── sema/                    # v0.4.0 - 语义分析
│   ├── mod.zig
│   ├── types.zig           # 类型定义
│   ├── type_provider.zig   # 类型工厂/缓存
│   ├── scope.zig           # 作用域管理
│   ├── resolver.zig        # 名称解析
│   └── type_checker.zig    # 类型检查
│
├── yul/                     # v0.5.0 - Yul IR
│   ├── mod.zig
│   ├── ast.zig             # Yul AST
│   ├── parser.zig          # Yul 解析器
│   ├── ir_generator.zig    # IR 生成器
│   ├── printer.zig         # Yul 打印
│   └── optimizer/          # v0.6.0 - 优化器
│       ├── mod.zig
│       └── passes/         # 优化通道
│
├── codegen/                 # v0.7.0 - 代码生成
│   ├── mod.zig
│   ├── opcodes.zig         # EVM 操作码
│   ├── stack.zig           # 栈管理
│   ├── assembler.zig       # 汇编器
│   └── evm_transform.zig   # Yul → EVM
│
├── abi/                     # v0.8.0 - ABI
│   ├── mod.zig
│   ├── encoder.zig         # ABI 编码
│   ├── decoder.zig         # ABI 解码
│   └── json.zig            # ABI JSON
│
├── stdlib/                  # v0.9.0 - 标准库
│   ├── mod.zig
│   ├── math.zig            # 数学函数
│   ├── crypto.zig          # 密码学
│   └── globals.zig         # 全局变量
│
├── interface/               # 公共接口
│   ├── mod.zig
│   ├── cli.zig             # 命令行接口
│   └── standard_json.zig   # JSON API
│
└── util/                    # 工具模块
    ├── mod.zig
    ├── error.zig           # 错误处理
    ├── diagnostics.zig     # 诊断输出
    └── allocator.zig       # 内存管理
```

---

## 依赖关系图

```
util (基础)
  │
  ▼
lexer (v0.1.0)
  │
  ▼
parser (v0.2.0)
  │
  ▼
ast (v0.3.0)
  │
  ├───────────────┐
  ▼               ▼
sema (v0.4.0)   json export
  │
  ▼
yul (v0.5.0)
  │
  ▼
optimizer (v0.6.0)
  │
  ▼
codegen (v0.7.0)
  │
  ▼
abi (v0.8.0)
  │
  ▼
stdlib (v0.9.0)
  │
  ▼
interface (v1.0.0)
```

---

## 风险评估

### 高风险区域

| 组件 | 风险 | 缓解策略 |
|------|------|----------|
| **类型系统** | 复杂度高 (~127K 行 C++) | 从核心类型开始，逐步扩展 |
| **类型检查器** | 规则复杂 (~149K 行 C++) | 使用官方测试套件验证 |
| **Yul 优化器** | 50+ 优化通道 | 先实现必要通道 |
| **EVM 栈管理** | 寄存器分配问题 | 研究现有算法 |

### 中等风险

| 组件 | 风险 | 缓解策略 |
|------|------|----------|
| **解析器** | 语法边缘情况 | 大量测试用例 |
| **存储布局** | 打包规则复杂 | 仔细遵循规范 |
| **继承解析** | 钻石继承、线性化 | 按 C3 线性化算法 |

### 低风险

| 组件 | 风险 | 缓解策略 |
|------|------|----------|
| **词法分析器** | 相对简单 | 状态机实现 |
| **ABI 编码** | 规范明确 | 直接实现规范 |
| **操作码定义** | 固定规范 | 直接映射 |

---

## 质量标准

### 测试要求

- **单元测试覆盖**: 每个模块 >80%
- **集成测试**: 端到端编译测试
- **回归测试**: Solidity 官方测试套件兼容
- **Fuzz 测试**: 词法分析器和解析器

### 文档要求

- 每个公共 API 有文档注释
- 每个模块有 README.md
- 设计决策记录在 docs/design/
- 更新日志在 CHANGELOG.md

### 代码质量

- 无内存泄漏 (`std.testing.allocator`)
- 无未处理错误
- 遵循 Zig 0.15 API
- 遵循 AGENTS.md 规范

---

## 参考资源

### 官方文档
- [Solidity 文档](https://docs.soliditylang.org/)
- [Solidity 语法](https://docs.soliditylang.org/en/latest/grammar.html)
- [ABI 规范](https://docs.soliditylang.org/en/latest/abi-spec.html)
- [EVM 操作码](https://ethereum.org/en/developers/docs/evm/opcodes/)

### 源码参考
- [ethereum/solidity](https://github.com/ethereum/solidity) (C++ 实现)
- [nomicfoundation/slang](https://github.com/nomicfoundation/slang) (Rust 解析器)
- [ziglang/zig](https://github.com/ziglang/zig) (Zig 编译器模式)

### 相关项目
- [Vexu/arocc](https://github.com/Vexu/arocc) (Zig C 编译器)
- [bluealloy/revm](https://github.com/bluealloy/revm) (Rust EVM)

---

## 时间线预估

| 阶段 | 版本 | 预计工期 | 累计 |
|------|------|----------|------|
| 词法分析 | v0.1.0 | 2-3 周 | 3 周 |
| 语法分析 | v0.2.0 | 3-4 周 | 7 周 |
| AST | v0.3.0 | 2-3 周 | 10 周 |
| 语义分析 | v0.4.0 | 4-6 周 | 16 周 |
| Yul IR | v0.5.0 | 3-4 周 | 20 周 |
| 优化器 | v0.6.0 | 4-5 周 | 25 周 |
| 代码生成 | v0.7.0 | 4-5 周 | 30 周 |
| ABI | v0.8.0 | 2-3 周 | 33 周 |
| 标准库 | v0.9.0 | 3-4 周 | 37 周 |
| 发布 | v1.0.0 | 4-6 周 | 43 周 |

**总预计**: 约 10-12 个月

---

## 下一步行动

1. **创建 v0.1.0 Story**: [stories/v0.1.0-lexer.md](stories/v0.1.0-lexer.md)
2. **设置模块结构**: 创建基础目录和模块文件
3. **实现 Token 定义**: 从 `src/lexer/token.zig` 开始
4. **运行测试**: 确保 `zig build test` 通过

---

*最后更新*: 2026-01-03  
*状态*: 规划阶段
