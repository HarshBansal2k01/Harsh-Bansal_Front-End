#  STEELEYE LIMITED FRONTEND ENGINEER ASSIGNMENT

# Q1. Explain what the simple List component does.

The simple List component is a React component that renders a list of items as an unordered list onto the webpage.The code contains two components which are `WrappedListComponent` and `WrappedSingleListItem`.Relation of component goes like, `WrappedListComponent` it's a parent component whereas `WrappedSingleListItem` is child component. Here in the code "items" is array of object and  each object contains an element "text" which is string type which rendered on the webpage

## Functioning of simple List component ##

* The `WrappedSingleListComponent` is a memoized functional component as it uses `memo` that displays a single item with green and red as background colour depending upon the isSelected boolean value.
  * memo :- It will only re-render the component if its props have changed. This can improve performance by preventing unnecessary re-renders.
* The `WrappedListComponent` generetares complete list of item using the child component `WrappedSingleListComponent`.

* The List component receives an array of objects named `items` as props 
* The item array mapped in `WrappedListComponent` inside ul tag
* Set of props are passed in `WrappedSingleListItem` component 
    * { key={index}, text={item.text} ,index={index},isSelected={setSelectedIndex === index} ,onClickHandler={() => handleClick(index)} }
* The Items are displayed on the webpage and whenever user click on a single list item it should change colour from red to green and vice versa.
* The `isSelected` boolean variable is use for changing the background color of the clicked list item to green if true or else red if false.
* The `onClickHandler()` function sets the index of the particular item using the 'useState' hook named as 'selectedIndex'.
* So Combining both `isSelected` and `onClickHandler` trigger the `selectedIndex` hook and the  variable is compared to all the mapped indices where it checks the indices if matched then it become green or else becomes red.


## Q2. What problems / warnings are there with code? ##

* The `useState` hook in the `WrappedListComponent` is used incorrectly. useState accepts an initial state and returns two values that is `current state` and a `function that updates the state`. Here, it should be `[selectedIndex, setSelectedIndex]` instead of `[setSelectedIndex, selectedIndex]`.
* There is incorrect type definition in `WrappedListComponent.propTypes` of `PropTypes.array` and `PropTypes.shapeOf` .
* In `SingleListItem` component does not have a `key` prop, which gives a warning because a distinct "key" is required when we use the array "map" function for mapping elements of the array to distinguish between them.
* The isSelected prop of the SingleListItem component is a boolean value that indicates whether the item is selected or not. However, the code is passed with  selectedIndex value, which is a  null. This will cause a type mismatch warning and may result in incorrect behavior of the component. So, changing `isSelected` prop of the `SingleListItem` component to be a function that compares the index with the selectedIndex value and returns a boolean.
* The onClickHandler props is assigned incorrectly, it should be called as a callback to be called when the list item is clicked.

##  Please fix, optimize, and/or modify the component as much as you think is necessary. ##

* Swapped the order of `setSelectedIndex` and `selectedIndex` in `useState` hook.
* Changed the definition of `PropTypes.array` and `PropTypes.shapeOf` to `PropTypes.array` and `PropTypes.shapeOf` in `WrappedListComponent.propTypes`
* Added a `key` in prop in `SingleListItem` component which is equal to  `index` for identifying uniquely
* The value of selectedIndex is assigned to “null” changed to -1 as in the beginning it will reflect that nothing is selected.
* onClickHandler prop changed from `onClick={onClickHandler(index)}` to  `onClick={() => onClickHandler(index)}`.

### CODE ###

```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      // added a key prop
      key={index}
      // changed isSelected to a function
      style={{ backgroundColor: isSelected(index) ? "green" : "red" }}
      // declared with arrow function
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  // changed isSelected type to function
  isSelected: PropTypes.func.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  // interchanged selectedIndex and setSelectedIndex (modified 1)
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(-1);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };



  return (
    <ul style={{ textAlign: "left" }}>

      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          // modified
          isSelected={(ind) => ind === selectedIndex}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  // array -> arrayOf and shapeOf -> shape
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  // passing empty arr
  items: [],
};

const List = memo(WrappedListComponent);

export default List;


```

**Name** : Harsh Bansal <br/>
**Reg No**. : 12012999 <br/>
**Course** : Btech (CSE) <br/>
**Email id** : ihbbro14@gmail.com <br/>
**Mobile No.** : 8812960476 <br/>
