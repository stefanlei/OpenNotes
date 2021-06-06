#### cargo 配置文件

`https://www.bookstack.cn/read/RustPrimer/cargo-detailed-cfg-cargo-detailed-cfg.md#关于测试`

##### 最小配置

```toml
[package]
name = "helloworld-tonic"
version = "0.1.0"
edition = "2018"
build = "build.rs"
```

##### 其他配置

```toml
[package]
# 软件包名称，如果需要在别的地方引用此软件包，请用此名称。
name ="hello_world"
# 当前版本号，这里遵循semver标准，也就是语义化版本控制标准。
version ="0.1.0"# the current version, obeying semver
# 软件所有作者列表
authors =["you@example.com"]
# 非常有用的一个字段，如果要自定义自己的构建工作流，
# 尤其是要调用外部工具来构建其他本地语言（C、C++、D等）开发的软件包时。
# 这时，自定义的构建流程可以使用rust语言，写在"build.rs"文件中。
build ="build.rs"
# 显式声明软件包文件夹内哪些文件被排除在项目的构建流程之外，
# 哪些文件包含在项目的构建流程中
exclude =["build/**/*.o","doc/**/*.html"]
include =["src/**/*","Cargo.toml"]
# 当软件包在向公共仓库发布时出现错误时，使能此字段可以阻止此错误。
publish =false
# 关于软件包的一个简短介绍。
description ="..."
# 下面这些字段标明了软件包仓库的更多信息
documentation ="..."
homepage ="..."
repository ="..."
# 顾名思义，此字段指向的文件就是传说中的ReadMe，
# 并且，此文件的内容最终会保存在注册表数据库中。
readme ="..."
# 用于分类和检索的关键词。
keywords =["...","..."]
# 软件包的许可证，必须是cargo仓库已列出的已知的标准许可证。
license ="..."
# 软件包的非标许可证书对应的文件路径。
license-file ="..."
```

##### 编译时额外的动作

如果要做编译时，额外的动作，可以自定义 `build.rs` 字段，然后在 crates 根目录下面，添加 build.rs ，里面可以写一写额外的代码。

```toml
[package]
name = "helloworld-tonic"
version = "0.1.0"
edition = "2018"
build = "build.rs"
```

