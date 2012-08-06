---
layout: post
title: "Sublime Tips"
description: "Sublime Tips"
category: tool
tags: [sublime,vim,tool]
---
{% include JB/setup %}

最近尝试了下sublime 编辑器, 记录下使用过程找到的一些技巧
### vim 模式
在
	Preferences：Settings - User
设置
{% highlight bash %}
{
    "ignored_packages": []
}
{% endhighlight %}

### 绑定jj 键为ESC
在
	Preferences: Key-bindings - User
设置以下内容
{% highlight bash %}

[
    { "keys": ["j","j"], "command": "exit_insert_mode",
        "context":
        [
            { "key": "setting.command_mode", "operand": false },
            { "key": "setting.is_widget", "operand": false }
        ]
    },

    { "keys": ["j","j"], "command": "hide_auto_complete", "context":
        [
            { "key": "auto_complete_visible", "operator": "equal", "operand": true }
        ]
        },

    { "keys": ["j","j"], "command": "vi_cancel_current_action", "context":
        [
            { "key": "setting.command_mode" },
            { "key": "selection_empty", "operator": "equal", "operand": true, "match_all": false },
            { "key": "vi_has_input_state" }
        ]
    }
]
{% endhighlight %}

