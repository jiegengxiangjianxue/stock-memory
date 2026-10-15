---
name: piliai_package_rename
description: 当修改 piliai 项目的包名、目录、JNI 或查找回归脚本时
type: fact
---
包名已从 com.example.pilinara 重命名为 com.example.piliai，涉及 namespace/applicationId、Rust JNI 函数名 Java_com_example_piliai_*、Kotlin 目录、AndroidManifest、应用名字符串等约 135 个文件。但原生库 libpilinara_native.so 名称保持不变，以确保 JNI 加载一致性；回归脚本路径已同步更新为 piliai。