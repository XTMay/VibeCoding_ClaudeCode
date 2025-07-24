# Lesson 2: Claude基础操作 | Hello World扩展 + Prompt输入 + 代码生成

## 🎯 课程目标

通过本课学习，你将掌握：
- Claude的基础操作界面和功能
- 如何编写有效的Prompt进行代码生成
- Hello World项目的扩展开发
- 代码优化和调试技巧
- 多语言代码生成实践

## 📋 课程大纲

### 1. Claude基础操作回顾 (15分钟)
- Claude界面功能详解
- 对话历史管理
- 代码块复制和执行
- 文件上传和分析功能

### 2. Prompt工程基础 (20分钟)
- 什么是有效的Prompt
- Prompt的基本结构
- 角色设定和上下文提供
- 约束条件和输出格式指定

### 3. Hello World项目扩展 (30分钟)
- 基础版本回顾
- 添加用户交互功能
- 多语言支持
- 时间和日期功能
- 配置文件读取

### 4. 代码生成实践 (25分钟)
- 函数生成
- 类和模块创建
- 测试代码生成
- 文档字符串生成

### 5. 课堂练习 (15分钟)
- 实时编程挑战
- Prompt优化练习

## 💻 实战项目：扩展版Hello World

### 项目结构
```
hello_world_extended/
├── main.py              # 主程序入口
├── greetings.py         # 问候语模块
├── config.py           # 配置管理
├── utils.py            # 工具函数
├── config.json         # 配置文件
└── requirements.txt    # 依赖包
```

### 核心代码实现

#### main.py - 主程序
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Hello World 扩展版 - 主程序
支持多语言、时间显示、用户交互等功能
"""

import sys
import json
from datetime import datetime
from greetings import GreetingManager
from config import ConfigManager
from utils import get_user_input, display_menu

def main():
    """主函数"""
    print("=" * 50)
    print("🌟 欢迎使用 Hello World 扩展版 🌟")
    print("=" * 50)
    
    # 初始化配置和问候语管理器
    config_manager = ConfigManager()
    greeting_manager = GreetingManager(config_manager)
    
    while True:
        try:
            # 显示菜单
            display_menu()
            
            # 获取用户选择
            choice = get_user_input("请选择操作 (1-5): ", "int", range(1, 6))
            
            if choice == 1:
                # 基础问候
                name = get_user_input("请输入您的姓名: ", "str")
                greeting = greeting_manager.get_greeting(name)
                print(f"\n{greeting}\n")
                
            elif choice == 2:
                # 多语言问候
                name = get_user_input("请输入您的姓名: ", "str")
                lang = get_user_input("选择语言 (zh/en/es/fr/ja): ", "str")
                greeting = greeting_manager.get_multilingual_greeting(name, lang)
                print(f"\n{greeting}\n")
                
            elif choice == 3:
                # 时间问候
                name = get_user_input("请输入您的姓名: ", "str")
                greeting = greeting_manager.get_time_based_greeting(name)
                print(f"\n{greeting}\n")
                
            elif choice == 4:
                # 查看配置
                config_manager.display_config()
                
            elif choice == 5:
                # 退出程序
                print("\n👋 感谢使用，再见！")
                break
                
        except KeyboardInterrupt:
            print("\n\n👋 程序被用户中断，再见！")
            break
        except Exception as e:
            print(f"\n❌ 发生错误: {e}")
            print("请重试...\n")

if __name__ == "__main__":
    main()
```

#### greetings.py - 问候语模块
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
问候语管理模块
提供多语言、时间相关的问候功能
"""

import random
from datetime import datetime
from typing import Dict, List

class GreetingManager:
    """问候语管理器"""
    
    def __init__(self, config_manager):
        self.config = config_manager
        self.greetings = {
            'zh': {
                'morning': ['早上好', '早安', '上午好'],
                'afternoon': ['下午好', '午安'],
                'evening': ['晚上好', '晚安'],
                'default': ['你好', '您好', '欢迎']
            },
            'en': {
                'morning': ['Good morning', 'Morning', 'Top of the morning'],
                'afternoon': ['Good afternoon', 'Afternoon'],
                'evening': ['Good evening', 'Evening'],
                'default': ['Hello', 'Hi', 'Welcome']
            },
            'es': {
                'morning': ['Buenos días', 'Buen día'],
                'afternoon': ['Buenas tardes'],
                'evening': ['Buenas noches'],
                'default': ['Hola', 'Bienvenido']
            },
            'fr': {
                'morning': ['Bonjour', 'Bon matin'],
                'afternoon': ['Bon après-midi'],
                'evening': ['Bonsoir', 'Bonne soirée'],
                'default': ['Bonjour', 'Salut', 'Bienvenue']
            },
            'ja': {
                'morning': ['おはようございます', 'おはよう'],
                'afternoon': ['こんにちは'],
                'evening': ['こんばんは'],
                'default': ['こんにちは', 'いらっしゃいませ']
            }
        }
    
    def get_greeting(self, name: str) -> str:
        """获取基础问候语"""
        default_lang = self.config.get_setting('default_language', 'zh')
        greetings = self.greetings[default_lang]['default']
        greeting = random.choice(greetings)
        return f"{greeting}, {name}! 🎉"
    
    def get_multilingual_greeting(self, name: str, language: str = 'zh') -> str:
        """获取多语言问候语"""
        if language not in self.greetings:
            language = 'zh'
        
        greetings = self.greetings[language]['default']
        greeting = random.choice(greetings)
        
        # 添加语言特定的装饰
        decorations = {
            'zh': '🇨🇳',
            'en': '🇺🇸',
            'es': '🇪🇸',
            'fr': '🇫🇷',
            'ja': '🇯🇵'
        }
        
        decoration = decorations.get(language, '🌍')
        return f"{greeting}, {name}! {decoration}"
    
    def get_time_based_greeting(self, name: str) -> str:
        """获取基于时间的问候语"""
        now = datetime.now()
        hour = now.hour
        
        # 确定时间段
        if 5 <= hour < 12:
            time_period = 'morning'
        elif 12 <= hour < 18:
            time_period = 'afternoon'
        else:
            time_period = 'evening'
        
        default_lang = self.config.get_setting('default_language', 'zh')
        greetings = self.greetings[default_lang][time_period]
        greeting = random.choice(greetings)
        
        # 添加时间信息
        time_str = now.strftime("%Y-%m-%d %H:%M")
        return f"{greeting}, {name}! ⏰ 现在是 {time_str}"
```

#### config.py - 配置管理
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
配置管理模块
处理应用程序配置的读取和管理
"""

import json
import os
from typing import Any, Dict

class ConfigManager:
    """配置管理器"""
    
    def __init__(self, config_file: str = 'config.json'):
        self.config_file = config_file
        self.config = self._load_config()
    
    def _load_config(self) -> Dict[str, Any]:
        """加载配置文件"""
        default_config = {
            'app_name': 'Hello World Extended',
            'version': '2.0.0',
            'default_language': 'zh',
            'show_time': True,
            'max_name_length': 50,
            'greeting_style': 'friendly'
        }
        
        if os.path.exists(self.config_file):
            try:
                with open(self.config_file, 'r', encoding='utf-8') as f:
                    loaded_config = json.load(f)
                    # 合并默认配置和加载的配置
                    default_config.update(loaded_config)
            except (json.JSONDecodeError, FileNotFoundError) as e:
                print(f"⚠️  配置文件读取失败: {e}")
                print("使用默认配置...")
        
        return default_config
    
    def get_setting(self, key: str, default: Any = None) -> Any:
        """获取配置项"""
        return self.config.get(key, default)
    
    def set_setting(self, key: str, value: Any) -> None:
        """设置配置项"""
        self.config[key] = value
        self._save_config()
    
    def _save_config(self) -> None:
        """保存配置到文件"""
        try:
            with open(self.config_file, 'w', encoding='utf-8') as f:
                json.dump(self.config, f, ensure_ascii=False, indent=2)
        except Exception as e:
            print(f"❌ 配置保存失败: {e}")
    
    def display_config(self) -> None:
        """显示当前配置"""
        print("\n📋 当前配置:")
        print("-" * 30)
        for key, value in self.config.items():
            print(f"{key}: {value}")
        print("-" * 30 + "\n")
```

#### utils.py - 工具函数
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
工具函数模块
提供通用的辅助函数
"""

from typing import Any, Union, List, Range

def get_user_input(prompt: str, input_type: str = 'str', valid_range: Range = None) -> Any:
    """获取用户输入并进行类型转换和验证"""
    while True:
        try:
            user_input = input(prompt).strip()
            
            if input_type == 'str':
                if not user_input:
                    print("❌ 输入不能为空，请重试")
                    continue
                return user_input
            
            elif input_type == 'int':
                value = int(user_input)
                if valid_range and value not in valid_range:
                    print(f"❌ 请输入 {min(valid_range)} 到 {max(valid_range)} 之间的数字")
                    continue
                return value
            
            elif input_type == 'float':
                return float(user_input)
            
            else:
                return user_input
                
        except ValueError:
            print(f"❌ 输入格式错误，请输入有效的{input_type}类型")
        except KeyboardInterrupt:
            raise
        except Exception as e:
            print(f"❌ 输入处理错误: {e}")

def display_menu() -> None:
    """显示主菜单"""
    menu_items = [
        "1. 🎯 基础问候",
        "2. 🌍 多语言问候", 
        "3. ⏰ 时间问候",
        "4. ⚙️  查看配置",
        "5. 🚪 退出程序"
    ]
    
    print("\n" + "=" * 30)
    print("📋 功能菜单")
    print("=" * 30)
    for item in menu_items:
        print(item)
    print("=" * 30)

def format_output(text: str, style: str = 'info') -> str:
    """格式化输出文本"""
    styles = {
        'info': '💡',
        'success': '✅',
        'warning': '⚠️',
        'error': '❌',
        'question': '❓'
    }
    
    icon = styles.get(style, '📝')
    return f"{icon} {text}"
```

#### config.json - 配置文件
```json
{
  "app_name": "Hello World Extended",
  "version": "2.0.0",
  "default_language": "zh",
  "show_time": true,
  "max_name_length": 50,
  "greeting_style": "friendly",
  "supported_languages": ["zh", "en", "es", "fr", "ja"],
  "time_format": "%Y-%m-%d %H:%M:%S"
}
```

#### requirements.txt - 依赖包
```txt
# Hello World Extended 项目依赖
# Python 3.12+ required

# 暂无外部依赖，使用Python标准库
# 如需添加依赖，请在此处列出
```

## 🎯 课堂练习

### 练习1：Prompt优化 (10分钟)
请使用Claude生成以下功能的代码：
1. 一个计算器类，支持基本四则运算
2. 包含错误处理和输入验证
3. 添加历史记录功能

### 练习2：代码扩展 (15分钟)
基于提供的Hello World项目：
1. 添加一个新的问候语言（如韩语、德语）
2. 实现问候语的个性化定制功能
3. 添加用户偏好设置保存功能

## 📝 课后作业

### 作业1：个人信息管理器
创建一个简单的个人信息管理程序：
- 支持添加、查看、修改个人信息
- 数据持久化存储（JSON文件）
- 信息验证和格式化
- 友好的用户界面

### 作业2：Prompt模板创建
为以下场景创建Prompt模板：
1. 代码重构建议
2. 性能优化分析
3. 安全漏洞检查
4. 代码文档生成

## 🔗 补充资源

### 推荐阅读
- [Python官方文档 - 模块和包](https://docs.python.org/3/tutorial/modules.html)
- [JSON处理最佳实践](https://realpython.com/working-with-json-data-python/)
- [Python异常处理指南](https://docs.python.org/3/tutorial/errors.html)

### 在线工具
- [Python代码格式化工具](https://black.vercel.app/)
- [JSON验证器](https://jsonlint.com/)
- [Python类型检查](https://mypy-lang.org/)

---

**下节预告**: Lesson 3 - AI简历优化器 | 简历分析工具 | 文本解析 + 匹配优化建议