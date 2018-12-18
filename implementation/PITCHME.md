@title[Implementation]
API Implementation
---
@snap[north]
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
@snap[north]
Remove the hierarchy structure from the collection
@snapend
```json
{
    "hierarchyId": "EFGH-5678-E5F6",
    ...otherData
}
```
---
@snap[north span-70]
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
@snap[north span-70]
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
@snap[midpoint span-90]
Now the data in the hierarchy collection can queried to return `hierarchyIds`, 
which can then be used to return all the products as an array
@snapend
---
UI Implementation
---
Get all base products
```javascript
const getAllProducts = state.products;
const getBaseProducts = 
    createSelector(getAllProducts, products => {
        return products.filter(p => !p.parentId);
    });
```
---
Create a memoized selector
```javascript
const getAllProducts = state.products;
const groupProducts = createSelector(getAllProducts, 
    products => products.reduce((groupedIds, prod) => {
        groupedIds[prod.parentId] = groupedIds[prod.parentId] || [];
        groupedIds[prod.parentId].push(prod);
        return groupedIds;
    )
}, {})
const getChildren = productId => 
    createSelector(groupProducts, 
        groupedProducts => groupedProducts[productId]);
```
---
@title[Implementation 2]
Create a reversible component
with React Component arrays
```javascript
class ReversibleHierarchy extends React.PureComponent {
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