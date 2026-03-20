# 🔧 LPMM Knowledge Base Import File Requirements

## OpenIE File Naming Requirements

After extraction is complete, OpenIE files will be named `月-日-时-分-openie.json`, stored in `data/openie` directory.

At this time, you can rename, for example add content description.

But, the `-openie.json` suffix at the end of the file must be retained.

Examples:

Valid file names:

- `千恋万花剧情-openie.json`
- `明日方舟全剧情-openie.json`
- `网络热梗(截止到2023年10月)-openie.json`

Invalid file names:
- `贴吧热梗-openie.txt` (non-json format)
- `明日方舟全剧情.json` (no openie identifier)
- `114514-openie.json` (content description not precise)

## 一、OpenIE Technology Overview
**Open Information Extraction (OpenIE)** is an open domain information extraction technology that aims to automatically extract structured relation triples (`subject-predicate-object`) from unstructured text, without needing to predefine relation types. Its core characteristics include:
- **Unsupervised**: Doesn't rely on predefined domain ontologies or relation libraries.
- **Flexibility**: Can handle diverse language expressions (e.g., "Apple founded by Steve Jobs" and "Steve Jobs is Apple's founder").
- **Redundancy tolerance**: Allows multiple extractions of same relation different expressions.

## 二、OpenIE Data Format Specification
### 1. Overall Structure
```json
{
    "docs": [
        {
            "idx": "Document's unique identifier (usually text's SHA256 hash value)",
            "passage": "Document's original text",
            "extracted_entities": ["entity1", "entity2", ...],
            "extracted_triples": [["subject", "predicate", "object"], ...]
        },
        ...
    ],
    "avg_ent_chars": "Entity average character count",
    "avg_ent_words": "Entity average word count"
}
```

### 2. Field Explanation
| Field                | Type       | Description                                                                 |
|---------------------|------------|----------------------------------------------------------------------|
| `docs`              | Array       | Contains all documents' extraction results                                               |
| `idx`               | String     | Document unique identifier (usually uses SHA256 hash value to ensure text uniqueness)                   |
| `passage`           | String     | Original text content                                                         |
| `extracted_entities`| String array | All entities identified from text (deduplicated)                                     |
| `extracted_triples` | Triple array | Extracted structured relations, each triple format `["subject", "predicate", "object"]`        |
| `avg_ent_chars`     | Numeric       | Entity average character length (statistics all `extracted_entities`)                   |
| `avg_ent_words`     | Numeric       | Entity average word count (calculated by space word segmentation)                                   |

### 3. Example
```json
{
    "docs": [
        {
            "idx": "a1b2c3...",
            "passage": "Steve Jobs founded Apple in 1976.",
            "extracted_entities": ["Steve Jobs", "Apple", "1976"],
            "extracted_triples": [
                ["Steve Jobs", "founded", "Apple"],
                ["Apple", "founded in", "1976"]
            ]
        }
    ],
    "avg_ent_chars": 8.3,
    "avg_ent_words": 1.7
}
```