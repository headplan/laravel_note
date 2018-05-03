# 配置流程

#### 配置conposer.json

```
"name": "headplan/lartisan",
"description": "Lartisan.",
"keywords": ["lartisan", "headplan"],
```

#### 配置.gitignore文件

```
/node_modules
/public/hot
/public/storage
/storage/*.key
/vendor
/.idea
/.vagrant
Homestead.json
Homestead.yaml
npm-debug.log
yarn-error.log
.env
.DS_Store
```

#### 添加编辑器代码风格配置

.editorconfig

```
# coding styles between different editors and IDEs
# editorconfig.org

root = true

[*]

# Change these settings to your own preference
indent_style = space
indent_size = 4

# We recommend you to keep these unchanged
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = false

[*.{js,html,blade.php,css,scss}]
indent_style = space
indent_size = 4

[*.md]
trim_trailing_whitespace = false
```



