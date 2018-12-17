@title[Situation]

Current Implementation

---
Product Collection MongoDB schema

```json
{
    "productId": "ABCD-1234-A1B2",
    "hierarchyId": "EFGH-5678-E5F6",
    ...otherData
}
```

---
Hierarchy Collection MongoDB schema
```json
{
    "hierarchyId": "EFGH-5678-E5F6",
    "productId": "ABCD-1234-A1B2",
    "children": [{
        "productId": "BCDE-2345-B2C3",
        "children": [{
            "productId": "DEFG-4567-D5E6",
            "children": [
                ...more children
            ]
        }]
    },
    ...more children
    ],
    ...otherData
}
```
---
UI View Example
---
Challenge
---
Issues with Current Implementation
@ul
- Hierarchy document difficult to maintain
- Building the hierarchy on the fly led to long wait times on the GET endpoint
- UI Component structure was built specifically for one crop type
@ulend
