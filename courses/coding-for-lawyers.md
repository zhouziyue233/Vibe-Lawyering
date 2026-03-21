# 💻 编程入门课程详细内容

> 法律人的编程实战指南，所有案例均来自真实法律工作场景。
> 学习目标：能够用代码处理法律文本，提升工作效率，而不是成为软件工程师。

---

## Python 入门

### 为什么法律人要学 Python？

Python 是最适合法律人的编程语言，原因如下：

- **语法简单**：接近自然语言，学习曲线平缓
- **文本处理强**：天生适合处理合同、判决书等文本数据
- **库生态丰富**：读取 PDF、Word、Excel 都有现成工具
- **AI 集成方便**：直接调用 Claude/GPT API 处理法律文本

### 环境搭建（15分钟）

```bash
# 1. 下载安装 Python 3.11+
# 访问 https://www.python.org/downloads/

# 2. 验证安装
python --version   # 应显示 Python 3.x.x

# 3. 安装常用法律工具包
pip install python-docx PyPDF2 openpyxl pandas anthropic
```

### 基础语法：法律场景速成

#### 变量和数据类型

```python
# 合同基本信息
contract_name = "技术服务协议"          # 字符串（文本）
contract_amount = 500000                # 整数
contract_rate = 0.06                    # 浮点数（比例）
is_signed = True                        # 布尔值（是/否）

# 列表：多个当事人
parties = ["甲方：ABC科技有限公司", "乙方：XYZ律师事务所"]

# 字典：合同要素
contract_info = {
    "名称": "技术服务协议",
    "金额": 500000,
    "期限": "12个月",
    "签署日期": "2026-01-15"
}

print(contract_info["金额"])  # 输出：500000
```

#### 循环：批量处理

```python
# 批量打印当事人信息
for party in parties:
    print(f"当事人：{party}")

# 输出：
# 当事人：甲方：ABC科技有限公司
# 当事人：乙方：XYZ律师事务所
```

#### 函数：封装重复操作

```python
def calculate_penalty(contract_amount, penalty_rate, delay_days):
    """计算违约金"""
    daily_penalty = contract_amount * penalty_rate
    total_penalty = daily_penalty * delay_days
    return total_penalty

# 使用
penalty = calculate_penalty(500000, 0.001, 30)
print(f"违约金总额：{penalty:,.0f} 元")  # 输出：违约金总额：15,000 元
```

---

## 批量处理合同

### 实战项目：合同台账自动生成

**目标：** 读取一个文件夹中的所有 Word 合同，自动提取关键信息，生成 Excel 台账。

#### 项目结构

```
my_contract_tool/
├── contracts/          # 放入所有合同 Word 文件
├── extract_info.py     # 主程序
└── output/             # 生成的台账文件
```

#### 完整代码

```python
import os
from docx import Document
import pandas as pd
import re

def extract_contract_info(file_path):
    """从 Word 文档中提取合同基本信息"""
    try:
        doc = Document(file_path)
        full_text = "\n".join([para.text for para in doc.paragraphs])

        info = {
            "文件名": os.path.basename(file_path),
            "合同名称": "",
            "甲方": "",
            "乙方": "",
            "合同金额": "",
            "合同期限": "",
        }

        # 提取合同名称（通常在前5行）
        first_lines = [p.text.strip() for p in doc.paragraphs[:5] if p.text.strip()]
        if first_lines:
            info["合同名称"] = first_lines[0]

        # 提取金额（寻找"人民币X元"或"¥X"格式）
        amount_pattern = r'(?:人民币|¥|金额为)[^\d]*(\d[\d,]*(?:\.\d+)?)\s*(?:元|万元)?'
        amount_match = re.search(amount_pattern, full_text)
        if amount_match:
            info["合同金额"] = amount_match.group(1)

        # 提取甲方乙方
        for line in full_text.split('\n'):
            if '甲方' in line and '：' in line and not info["甲方"]:
                info["甲方"] = line.split('：', 1)[1].strip()
            if '乙方' in line and '：' in line and not info["乙方"]:
                info["乙方"] = line.split('：', 1)[1].strip()

        return info

    except Exception as e:
        return {"文件名": os.path.basename(file_path), "错误": str(e)}

def generate_contract_ledger(contracts_folder, output_file):
    """批量处理合同，生成 Excel 台账"""
    all_contracts = []

    # 遍历文件夹中所有 docx 文件
    for filename in os.listdir(contracts_folder):
        if filename.endswith('.docx') and not filename.startswith('~'):
            file_path = os.path.join(contracts_folder, filename)
            print(f"正在处理：{filename}")
            contract_info = extract_contract_info(file_path)
            all_contracts.append(contract_info)

    # 生成 DataFrame 并导出 Excel
    df = pd.DataFrame(all_contracts)
    df.to_excel(output_file, index=False)
    print(f"\n✅ 台账已生成：{output_file}")
    print(f"共处理合同 {len(all_contracts)} 份")

    return df

# 运行
if __name__ == "__main__":
    generate_contract_ledger(
        contracts_folder="./contracts",
        output_file="./output/合同台账.xlsx"
    )
```

#### 运行方法

```bash
# 1. 将你的合同 Word 文件放入 contracts/ 文件夹
# 2. 运行脚本
python extract_info.py
# 3. 在 output/ 文件夹中查看生成的台账
```

---

## 数据可视化

### 实战：裁判文书胜诉率分析

```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 设置中文字体（macOS）
plt.rcParams['font.sans-serif'] = ['PingFang SC', 'SimHei']
plt.rcParams['axes.unicode_minus'] = False

# 示例数据（实际使用时从裁判文书数据库导出）
data = {
    '案由': ['合同纠纷', '劳动争议', '房屋租赁', '知识产权', '公司股权'],
    '原告胜诉': [245, 189, 156, 78, 43],
    '被告胜诉': [123, 201, 89, 34, 67],
    '调解/撤诉': [89, 156, 45, 23, 12],
}

df = pd.DataFrame(data)
df['总计'] = df['原告胜诉'] + df['被告胜诉'] + df['调解/撤诉']
df['原告胜诉率'] = (df['原告胜诉'] / df['总计'] * 100).round(1)

# 绘制横向条形图
fig, ax = plt.subplots(figsize=(10, 6))
bars = ax.barh(df['案由'], df['原告胜诉率'], color='#2E86AB', edgecolor='white')

# 添加数据标签
for bar, rate in zip(bars, df['原告胜诉率']):
    ax.text(bar.get_width() + 0.5, bar.get_y() + bar.get_height()/2,
            f'{rate}%', va='center', fontsize=11)

ax.set_xlabel('原告胜诉率 (%)', fontsize=12)
ax.set_title('各类型案件原告胜诉率分析\n（样本数据，仅供演示）', fontsize=14, pad=15)
ax.set_xlim(0, 85)
ax.grid(axis='x', alpha=0.3)

plt.tight_layout()
plt.savefig('胜诉率分析.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## 构建法律工具

### 实战：合同批量审查工具（调用 Claude API）

```python
import anthropic
import os
from docx import Document

def review_contract_with_ai(contract_text: str, party: str = "乙方") -> str:
    """
    使用 Claude API 审查合同
    :param contract_text: 合同全文
    :param party: 委托方身份（甲方/乙方）
    :return: 审查报告
    """
    client = anthropic.Anthropic(
        api_key=os.environ.get("ANTHROPIC_API_KEY")
    )

    system_prompt = """你是一名拥有15年经验的资深商事律师，专门从事合同审查工作。
    当收到合同文本时，你需要：
    1. 识别对委托方不利的风险条款
    2. 按高/中/低对风险分级
    3. 对每条风险给出修改建议
    4. 给出总体评估"""

    user_message = f"""委托方身份：{party}

请审查以下合同：

{contract_text[:50000]}  # 限制长度避免超出 token 限制
"""

    message = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=4000,
        system=system_prompt,
        messages=[{"role": "user", "content": user_message}]
    )

    return message.content[0].text

def batch_review(contracts_folder: str, output_folder: str):
    """批量审查合同并生成报告"""
    os.makedirs(output_folder, exist_ok=True)

    for filename in os.listdir(contracts_folder):
        if not filename.endswith('.docx'):
            continue

        print(f"正在审查：{filename}")

        # 读取合同文本
        doc = Document(os.path.join(contracts_folder, filename))
        contract_text = "\n".join([p.text for p in doc.paragraphs])

        # 调用 AI 审查
        review_result = review_contract_with_ai(contract_text)

        # 保存报告
        output_path = os.path.join(output_folder, f"审查报告_{filename}.txt")
        with open(output_path, 'w', encoding='utf-8') as f:
            f.write(f"合同文件：{filename}\n")
            f.write("=" * 50 + "\n")
            f.write(review_result)

        print(f"  ✅ 报告已保存至：{output_path}")

# 运行
if __name__ == "__main__":
    # 设置你的 API Key
    os.environ["ANTHROPIC_API_KEY"] = "your-api-key-here"

    batch_review(
        contracts_folder="./contracts",
        output_folder="./review_reports"
    )
```

### 重要说明

> ⚠️ **AI 审查的局限性：**
> - AI 的分析仅供参考，不构成正式法律意见
> - 对高风险合同，仍需执业律师进行专业审查
> - 请确保上传合同前已获得授权，注意数据保密
> - AI 可能存在"幻觉"，请验证引用的法条是否准确

---

## 下一步学习建议

完成以上课程后，推荐进阶方向：

1. **Web 开发**：用 Flask/Streamlit 把你的法律工具做成 Web 应用
2. **数据库**：用 SQLite 管理你的合同台账
3. **自动化**：用 Python 自动发送合同台账报告邮件
4. **机器学习**：用 sklearn 对裁判文书进行分类分析

推荐资源：[courses/README.md](README.md#外部优质资源推荐) 中的进阶课程列表
