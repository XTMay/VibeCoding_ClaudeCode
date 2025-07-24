#### Lesson 4: Prompt工程基础

```markdown:/Users/xiaotingzhou/Documents/Lectures/VibeCoding_ClaudeCode/lesson-04-prompt-engineering/README.md
# Lesson 4: Prompt工程基础 | 模板库构建 + 提示词设计优化技巧

## 🎯 课程目标
- 深入理解Prompt工程的核心原理
- 掌握高效Prompt设计的方法论
- 构建可复用的Prompt模板库
- 学会Prompt优化和调试技巧

## 📖 课程内容

### 1. Prompt工程理论基础 (15分钟)

#### 什么是Prompt工程
Prompt工程是设计和优化输入提示词的艺术和科学，目标是引导AI模型产生期望的输出结果。

#### 核心原理
1. **明确性原理**：清晰、具体的指令
2. **上下文原理**：提供充分的背景信息
3. **结构化原理**：有序的信息组织
4. **示例原理**：通过例子引导输出格式
5. **迭代原理**：持续优化和改进

#### Prompt的组成要素
```
[角色设定] + [任务描述] + [输入数据] + [输出要求] + [约束条件] + [示例]
```

### 2. 高效Prompt设计方法 (20分钟)

#### CLEAR框架
- **C**ontext (上下文)：提供背景信息
- **L**ength (长度)：控制输出长度
- **E**xamples (示例)：给出期望格式
- **A**udience (受众)：明确目标用户
- **R**ole (角色)：设定AI角色

#### 实践示例：代码审查Prompt

**基础版本：**
```
审查这段代码
```

**优化版本：**
```
你是一位拥有10年经验的Python高级工程师。请对以下代码进行专业审查：

代码内容：
```python
[代码]
```

审查要求：
1. 代码质量和可读性
2. 性能优化机会
3. 安全性问题
4. 最佳实践符合度
5. 潜在的bug风险

输出格式：
- 总体评分 (1-10)
- 具体问题列表
- 改进建议
- 优化后的代码示例

请保持专业、客观的评价态度。
```

### 3. 模板库构建实战 (10分钟)

#### 模板分类体系
```python
class PromptTemplateLibrary:
    def __init__(self):
        self.templates = {
            "code_generation": {
                "basic": self.basic_code_template,
                "advanced": self.advanced_code_template,
                "debugging": self.debugging_template
            },
            "analysis": {
                "code_review": self.code_review_template,
                "performance": self.performance_analysis_template,
                "security": self.security_audit_template
            },
            "documentation": {
                "api_docs": self.api_documentation_template,
                "readme": self.readme_template,
                "comments": self.code_comments_template
            }
        }
    
    def basic_code_template(self, language, functionality, requirements):
        return f"""
        你是一位专业的{language}开发者。请创建一个程序实现以下功能：
        
        功能描述：{functionality}
        
        技术要求：
        {chr(10).join(f'- {req}' for req in requirements)}
        
        请提供：
        1. 完整的可运行代码
        2. 详细的注释说明
        3. 使用示例
        4. 可能的改进建议
        """
    
    def advanced_code_template(self, language, functionality, architecture, constraints):
        return f"""
        作为资深{language}架构师，请设计并实现以下系统：
        
        系统功能：{functionality}
        架构要求：{architecture}
        约束条件：{constraints}
        
        请提供：
        1. 系统架构设计
        2. 核心模块实现
        3. 接口定义
        4. 测试策略
        5. 部署方案
        
        注重代码质量、可扩展性和维护性。
        """
```

#### 动态模板生成器
```python
class DynamicPromptGenerator:
    def __init__(self):
        self.base_templates = {
            "role_prefix": "你是一位{expertise}的{role}，拥有{experience}年经验。",
            "task_description": "请{action}{target}，{specific_requirements}。",
            "output_format": "请以{format}格式输出，包含{components}。",
            "constraints": "约束条件：{constraints}",
            "examples": "参考示例：{examples}"
        }
    
    def generate_prompt(self, template_config):
        """根据配置动态生成Prompt"""
        prompt_parts = []
        
        # 角色设定
        if 'role' in template_config:
            role_part = self.base_templates['role_prefix'].format(
                **template_config['role']
            )
            prompt_parts.append(role_part)
        
        # 任务描述
        if 'task' in template_config:
            task_part = self.base_templates['task_description'].format(
                **template_config['task']
            )
            prompt_parts.append(task_part)
        
        # 输出格式
        if 'output' in template_config:
            output_part = self.base_templates['output_format'].format(
                **template_config['output']
            )
            prompt_parts.append(output_part)
        
        return "\n\n".join(prompt_parts)
```

### 4. Prompt优化技巧 (15分钟)

#### A/B测试方法
```python
class PromptOptimizer:
    def __init__(self, claude_client):
        self.claude = claude_client
        self.test_results = []
    
    def ab_test_prompts(self, prompt_a, prompt_b, test_inputs, evaluation_criteria):
        """对比测试两个Prompt的效果"""
        results_a = []
        results_b = []
        
        for test_input in test_inputs:
            # 测试Prompt A
            response_a = self.claude.ask(prompt_a.format(input=test_input))
            score_a = self.evaluate_response(response_a, evaluation_criteria)
            results_a.append(score_a)
            
            # 测试Prompt B
            response_b = self.claude.ask(prompt_b.format(input=test_input))
            score_b = self.evaluate_response(response_b, evaluation_criteria)
            results_b.append(score_b)
        
        return {
            "prompt_a_avg": sum(results_a) / len(results_a),
            "prompt_b_avg": sum(results_b) / len(results_b),
            "winner": "A" if sum(results_a) > sum(results_b) else "B",
            "detailed_results": {
                "prompt_a": results_a,
                "prompt_b": results_b
            }
        }
    
    def evaluate_response(self, response, criteria):
        """评估响应质量"""
        evaluation_prompt = f"""
        请评估以下AI响应的质量：
        
        响应内容：{response}
        
        评估标准：
        {chr(10).join(f'- {criterion}' for criterion in criteria)}
        
        请给出1-10分的评分，并简要说明理由。
        """
        
        eval_result = self.claude.ask(evaluation_prompt)
        # 提取分数（简化实现）
        import re
        score_match = re.search(r'(\d+)分', eval_result)
        return int(score_match.group(1)) if score_match else 5
```

#### 迭代优化流程
1. **基线建立**：创建初始Prompt版本
2. **问题识别**：分析输出质量问题
3. **假设形成**：提出改进假设
4. **变体测试**：创建优化版本
5. **效果评估**：对比测试结果
6. **迭代改进**：基于结果继续优化

## 🛠️ 课堂练习

### 练习1：Prompt重构
将以下简单Prompt重构为高质量版本：

**原始Prompt：**
```
写一个排序算法
```

**重构要求：**
- 明确算法类型和性能要求
- 指定编程语言和代码风格
- 要求详细注释和测试用例
- 包含复杂度分析

### 练习2：模板库设计
为以下场景设计Prompt模板：
1. API文档生成
2. 单元测试创建
3. 代码重构建议
4. 性能优化分析

### 练习3：效果对比测试
设计A/B测试，比较不同Prompt版本的效果。

## 📝 课后作业

### 基础任务：个人模板库
创建包含以下类别的Prompt模板库：
1. 代码生成类（5个模板）
2. 代码分析类（5个模板）
3. 文档生成类（3个模板）
4. 调试诊断类（3个模板）

### 进阶任务：智能模板系统
开发一个智能Prompt模板系统：
1. 根据任务类型自动选择模板
2. 支持模板参数化配置
3. 实现模板效果评估
4. 提供模板优化建议

### 挑战任务：Prompt优化工具
创建一个Prompt优化工具：
1. 自动分析Prompt质量
2. 提供改进建议
3. 支持A/B测试
4. 生成优化报告

---

**课程时长**：50分钟  
**难度等级**：⭐⭐⭐⭐☆ (中高级)  
**前置要求**：熟悉Claude基础操作
```