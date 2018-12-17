@title[Implementation]
API Implementation
---
@snap[north-west]
Remove the hierarchy structure from the collection
@snapend
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
@snap[north-west]
Remove the hierarchy structure from the collection
@snapend
```json
{
    "hierarchyId": "EFGH-5678-E5F6",
    ...otherData
}
```
---
@snap[south-east]
Add parentId to every product
@snapend
```json
{
    "productId": "ABCD-1234-A1B2",
    "hierarchyId": "EFGH-5678-E5F6",
    ...otherData
}
```
---
@snap[south-east]
Add parentId to every product
@snapend
```json
{
    "productId": "ABCD-1234-A1B2",
    "hierarchyId": "EFGH-5678-E5F6",
    "parentId": "BCDE-2345-B2C3",
    ...otherData
}
```
---
UI Implementation
---
Get all base products

SWITCH TO USE GROUP BY
```javascript
const getAllProducts = state.products;
const getBaseProducts = 
    createSelector(getAllProducts, products => {
        return products.filter(p => !p.parentId);
    });
```
---
Create a memoized selector
```
const getAllProducts = state.products;
const getChildren = productId => 
    createSelector(getAllProducts, products => {
        return products.filter(p => p.parentId === productId);
    });
```
---
@title[Implementation 2]
Create a reversible component
with React Component arrays
```javascript
class ReversibleHierarchy extends React.Component {
    render() {
        const {product, reverse} = this.props;
        const childComponents = [
            <Parent />,
            {getChildren(product.id).map(child => 
                <ReversibleHierarchy 
                    product={child} 
                    reverse={reverse} />)}
        ];
        ```
---
```javascript
        return <React.Fragment>
            {reverse 
                ? childComponents.reverse() 
                : childComponents}
        </React.Fragment>;
    }
}
```