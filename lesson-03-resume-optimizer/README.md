# Lesson 3: AI简历优化器 | 文本解析 + 匹配优化建议

## 🎯 课程目标
- 掌握文本解析和信息提取技术
- 学会使用Claude进行简历分析
- 实现智能优化建议生成
- 构建完整的简历优化工具

## 📖 课程内容

### 1. 简历分析基础 (15分钟)

#### 简历结构解析
- 个人信息提取
- 教育背景分析
- 工作经验解析
- 技能关键词识别
- 项目经历提取

#### 文本处理技术
```python
import re
import json
from dataclasses import dataclass
from typing import List, Dict, Optional

@dataclass
class PersonalInfo:
    name: str
    email: str
    phone: str
    location: str

@dataclass
class Education:
    degree: str
    major: str
    school: str
    graduation_year: str
    gpa: Optional[str] = None

@dataclass
class WorkExperience:
    title: str
    company: str
    duration: str
    responsibilities: List[str]
    achievements: List[str]

class ResumeParser:
    def __init__(self):
        self.email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
        self.phone_pattern = r'\b\d{3}[-.]?\d{3}[-.]?\d{4}\b'
        
    def extract_contact_info(self, text: str) -> PersonalInfo:
        """提取联系信息"""
        email = re.search(self.email_pattern, text)
        phone = re.search(self.phone_pattern, text)
        
        # 使用Claude进行更智能的信息提取
        claude_prompt = f"""
        从以下简历文本中提取个人信息：
        
        {text[:500]}  # 只取前500字符
        
        请以JSON格式返回：
        {{
            "name": "姓名",
            "email": "邮箱",
            "phone": "电话",
            "location": "地址"
        }}
        """
        
        return claude_prompt
```

### 2. Claude集成分析 (20分钟)

#### 智能简历评估
```python
class ResumeAnalyzer:
    def __init__(self, claude_client):
        self.claude = claude_client
        
    def analyze_resume(self, resume_text: str, target_job: str = None) -> Dict:
        """使用Claude分析简历"""
        analysis_prompt = f"""
        请作为专业的HR顾问，分析以下简历：
        
        简历内容：
        {resume_text}
        
        目标职位：{target_job or "通用分析"}
        
        请从以下维度进行分析：
        1. 整体结构和格式
        2. 内容完整性
        3. 关键词匹配度
        4. 经验相关性
        5. 技能突出程度
        
        请以JSON格式返回分析结果：
        {{
            "overall_score": 85,
            "structure_score": 90,
            "content_score": 80,
            "keyword_score": 75,
            "experience_score": 88,
            "skills_score": 82,
            "strengths": ["优势1", "优势2"],
            "weaknesses": ["不足1", "不足2"],
            "suggestions": ["建议1", "建议2"]
        }}
        """
        
        return self.claude.ask(analysis_prompt)
        
    def generate_improvements(self, analysis_result: Dict) -> List[str]:
        """生成具体改进建议"""
        improvement_prompt = f"""
        基于以下简历分析结果，生成具体的改进建议：
        
        分析结果：{json.dumps(analysis_result, ensure_ascii=False, indent=2)}
        
        请提供：
        1. 5个具体的改进建议
        2. 每个建议包含具体的修改方法
        3. 优先级排序（高、中、低）
        
        格式：
        - [高优先级] 建议内容：具体修改方法
        - [中优先级] 建议内容：具体修改方法
        """
        
        return self.claude.ask(improvement_prompt)
```

### 3. 优化建议生成 (10分钟)

#### 行业特定优化
```python
class IndustryOptimizer:
    def __init__(self):
        self.industry_keywords = {
            "技术": ["Python", "机器学习", "云计算", "微服务", "DevOps"],
            "金融": ["风险管理", "合规", "数据分析", "投资", "财务建模"],
            "市场营销": ["数字营销", "SEO", "内容营销", "用户增长", "数据驱动"]
        }
        
    def optimize_for_industry(self, resume_text: str, industry: str) -> str:
        """针对特定行业优化简历"""
        keywords = self.industry_keywords.get(industry, [])
        
        optimization_prompt = f"""
        请优化以下简历，使其更适合{industry}行业：
        
        原始简历：
        {resume_text}
        
        行业关键词：{', '.join(keywords)}
        
        优化要求：
        1. 突出相关技能和经验
        2. 调整描述语言更符合行业习惯
        3. 增加行业关键词的自然使用
        4. 保持真实性，不添加虚假信息
        
        请返回优化后的简历内容。
        """
        
        return optimization_prompt
```

### 4. 实战项目开发 (15分钟)

#### 完整的简历优化器
```python
class ResumeOptimizer:
    def __init__(self, claude_client):
        self.parser = ResumeParser()
        self.analyzer = ResumeAnalyzer(claude_client)
        self.industry_optimizer = IndustryOptimizer()
        
    def process_resume(self, resume_file: str, target_job: str = None, 
                      industry: str = None) -> Dict:
        """完整的简历处理流程"""
        # 1. 读取简历文件
        resume_text = self.read_resume_file(resume_file)
        
        # 2. 解析简历结构
        parsed_data = self.parser.parse_resume(resume_text)
        
        # 3. 分析简历质量
        analysis = self.analyzer.analyze_resume(resume_text, target_job)
        
        # 4. 生成改进建议
        improvements = self.analyzer.generate_improvements(analysis)
        
        # 5. 行业特定优化（如果指定）
        optimized_resume = None
        if industry:
            optimized_resume = self.industry_optimizer.optimize_for_industry(
                resume_text, industry
            )
        
        return {
            "original_resume": resume_text,
            "parsed_data": parsed_data,
            "analysis": analysis,
            "improvements": improvements,
            "optimized_resume": optimized_resume,
            "processing_time": datetime.now().isoformat()
        }
        
    def read_resume_file(self, file_path: str) -> str:
        """读取不同格式的简历文件"""
        if file_path.endswith('.pdf'):
            return self.extract_text_from_pdf(file_path)
        elif file_path.endswith('.docx'):
            return self.extract_text_from_docx(file_path)
        elif file_path.endswith('.txt'):
            with open(file_path, 'r', encoding='utf-8') as f:
                return f.read()
        else:
            raise ValueError("不支持的文件格式")
```

## 🛠️ 课堂练习

### 练习1：简历解析器
创建一个简历解析器，能够从文本中提取：
- 个人联系信息
- 教育背景
- 工作经验
- 技能列表

### 练习2：Claude分析集成
使用Claude API分析简历质量，生成评分和建议。

### 练习3：批量处理
实现批量简历处理功能，支持多个文件同时优化。

## 📝 课后作业

### 基础任务
完善简历优化器，添加以下功能：
1. 支持PDF和Word文档解析
2. 生成优化报告
3. 关键词密度分析
4. ATS友好性检查

### 进阶任务
1. 添加Web界面
2. 实现简历模板生成
3. 支持多语言简历
4. 集成求职网站API

### 挑战任务
创建一个完整的求职助手系统：
1. 简历优化
2. 职位匹配
3. 面试准备
4. 薪资谈判建议

---

**课程时长**：50分钟  
**难度等级**：⭐⭐⭐☆☆ (中级)  
**前置要求**：完成前两课的学习
```

```markdown:/Users/xiaotingzhou/Documents/Lectures/VibeCoding_ClaudeCode/lesson-03-resume-optimizer/prompts.md
# Lesson 3 - 简历优化器 Prompt模板集合

## 🎯 简历分析类Prompt

### 基础简历评估模板
```
请作为资深HR专家，全面评估以下简历：

简历内容：
[简历文本]

评估维度：
1. 整体结构和格式 (0-100分)
2. 内容完整性和相关性 (0-100分)
3. 语言表达和专业性 (0-100分)
4. 关键词优化程度 (0-100分)
5. ATS系统友好性 (0-100分)

请提供：
- 各维度具体评分和理由
- 3个主要优势
- 3个关键不足
- 5个具体改进建议
- 总体评级（A/B/C/D）
```

### 行业特定分析模板
```
针对{目标行业}行业，分析以下简历的匹配度：

简历内容：[简历文本]
目标职位：{具体职位}

分析要点：
1. 行业相关经验匹配度
2. 技能要求符合程度
3. 关键词覆盖情况
4. 职业发展路径合理性
5. 行业认知和理解深度

请提供：
- 匹配度评分 (0-100)
- 缺失的关键技能
- 需要强化的经验
- 行业特定优化建议
```

## 🔧 简历优化类Prompt

### 内容优化模板
```
请优化以下简历内容，使其更具吸引力和专业性：

原始内容：
[简历段落]

优化要求：
1. 使用动作词汇开头
2. 量化成果和影响
3. 突出核心价值
4. 符合STAR原则（情境、任务、行动、结果）
5. 保持简洁有力

请提供：
- 优化后的内容
- 修改说明
- 使用的优化技巧
```

### 关键词优化模板
```
为以下简历添加相关关键词，提高ATS通过率：

简历内容：[简历文本]
目标职位描述：[JD内容]

优化策略：
1. 识别JD中的核心关键词
2. 自然融入到简历中
3. 保持内容真实性
4. 避免关键词堆砌
5. 平衡关键词密度

请提供：
- 需要添加的关键词列表
- 具体插入位置建议
- 优化后的段落示例
```

## 📊 数据提取类Prompt

### 结构化信息提取模板
```
从以下简历中提取结构化信息：

简历文本：[简历内容]

请以JSON格式返回：
{
  "personal_info": {
    "name": "",
    "email": "",
    "phone": "",
    "location": "",
    "linkedin": ""
  },
  "education": [
    {
      "degree": "",
      "major": "",
      "school": "",
      "graduation_year": "",
      "gpa": ""
    }
  ],
  "experience": [
    {
      "title": "",
      "company": "",
      "duration": "",
      "responsibilities": [],
      "achievements": []
    }
  ],
  "skills": {
    "technical": [],
    "soft": [],
    "languages": []
  },
  "certifications": [],
  "projects": []
}
```

### 技能匹配分析模板
```
分析简历技能与职位要求的匹配情况：

简历技能：[从简历提取的技能]
职位要求：[JD中的技能要求]

匹配分析：
1. 完全匹配的技能
2. 部分匹配的技能
3. 缺失的关键技能
4. 额外的优势技能
5. 技能水平评估

请提供：
- 技能匹配度百分比
- 详细匹配报告
- 技能提升建议
- 学习资源推荐
```

## 🎨 格式优化类Prompt

### 简历格式改进模板
```
改进以下简历的格式和布局：

当前简历：[简历内容]

格式要求：
1. 清晰的层次结构
2. 一致的格式风格
3. 合理的空白使用
4. 重点信息突出
5. ATS系统兼容

请提供：
- 格式化后的简历
- 格式改进说明
- 视觉层次建议
```

### 长度优化模板
```
将以下简历内容压缩到合适长度：

原始内容：[冗长的简历内容]
目标长度：{1页/2页}

压缩原则：
1. 保留最相关信息
2. 合并相似经历
3. 精简描述语言
4. 突出核心成就
5. 删除冗余内容

请提供：
- 压缩后的内容
- 删除内容的理由
- 保留内容的优先级
```

## 🚀 创新功能类Prompt

### 个性化建议模板
```
基于以下信息，为求职者提供个性化建议：

个人背景：[教育、经验、技能]
职业目标：[目标职位、行业、发展方向]
当前挑战：[求职难点、技能缺口]

建议维度：
1. 简历优化策略
2. 技能提升计划
3. 求职渠道建议
4. 面试准备重点
5. 职业发展路径

请提供：
- 3个月行动计划
- 具体执行步骤
- 成功指标设定
```

### 竞争力分析模板
```
分析求职者在目标市场的竞争力：

求职者简历：[简历内容]
目标职位：[职位信息]
市场情况：[行业趋势、薪资水平]

竞争力评估：
1. 技能竞争力
2. 经验竞争力
3. 教育背景竞争力
4. 软技能竞争力
5. 市场稀缺性

请提供：
- 竞争力雷达图数据
- 优势劣势分析
- 差异化建议
- 市场定位策略
```

## 💡 使用技巧

### Prompt优化原则
1. **具体明确**：清楚说明分析维度和期望输出
2. **结构化**：使用列表和分类组织信息
3. **专业角色**：设定HR专家或行业专家角色
4. **量化要求**：要求具体的评分和数据
5. **实用导向**：关注可执行的建议

### 常用修饰词
- **专业的**：获得更权威的分析
- **详细的**：获得深入的解释
- **实用的**：获得可操作的建议
- **客观的**：获得中性的评估
- **创新的**：获得独特的优化思路
```