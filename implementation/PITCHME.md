### API Implementation
---
@title[Remove the Hierarchy]
@snap[north span-70]
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
@title[Remove the Hierarchy 2]
@snap[north span-70]
Remove the hierarchy structure from the collection
@snapend
```json
{
    "hierarchyId": "EFGH-5678-E5F6",
    ...otherData
}
```
---
@title[Add parentId]
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
@title[Add parentId 2]
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
@title[API Results]
@snap[midpoint span-90]
Now the data in the hierarchy collection can queried to return `hierarchyIds`, 
which can then be used to return all the products as an array
@snapend
---
### UI Implementation
---
@title[Get Base Products]
Get all base products
```javascript
const getAllProducts = state.products;
const getBaseProducts = 
    createSelector(getAllProducts, products => {
        return products.filter(p => !p.parentId);
    });
```
---
@title[Create Selector]
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
@title[Create UI Component]
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
@title[Create UI Component cont'd]
```javascript
        return <React.Fragment>
            {reverse 
                ? childComponents.reverse() 
                : childComponents}
        </React.Fragment>;
    }
}
```