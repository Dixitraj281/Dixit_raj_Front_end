Q1. What is the Simple List Component in React?

Ans: The Simple List Component is a React Component which takes an array of items as a prop and renders an unordered list of those items with the option to select a single item from the list.
Upon selection, the background color of the selected list item is set to green, to indicate current selection. The Simple List Component has two components namely  WrappedSingleListItem and WrappedListComponent.
The WrappedSingleListItem has four properties:

index: the index of the item in the items array
isSelected: a boolean that indicates whether the item is currently selected or not
onClickHandler: a callback function that gets called when the list item is clicked
text: the text content of the item
Whereas, the WrappedListComponent makes use of useState() and useEffect() hook to set the color of the selected list item.

Q2. What problems / warnings are there with code?

Ans:
1. In the WrappedSingleListItem, the onClickHandler is not being invoked properly. Instead of being called when the list item is clicked, the onClickHandler is being called immediately when the component is rendered. The correct code should be:
onClick={()=>onClickHandler(index)}

2. The proptypes for index and isSelected should be isRequired as well. The correct code should be:
WrappedSingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};
3. Incorrect syntax of useState() hook. The useState() hook returns an array of two elements where the first element is the state value and the second element is a function to update the state value. The array elements need to be swapped. The correct code should be:
const [selectedIndex, setSelectedIndex] = useState(0);
in the items.map() function, each child in a list should have a unique key prop. Also, the isSelected prop should be passed as a boolean, instead it is being passed as a selectedIndex state value. The correct code should be:
{items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={index===selectedIndex}
        />
      ))}
4. In the WrappedListComponent, the prop type for the items array is not defined correctly. Instead of PropTypes.array(PropTypes.shapeOf({...})), it should be PropTypes.arrayOf(PropTypes.shape({...})). The correct code should be:
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

Q3. Please fix, optimize, and/or modify the component as much as you think is necessary.
Ans: 
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={()=>onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState(0);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={index===selectedIndex}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: [{text: "Dixit Raj"}, {text: "12006314"}, {text: "B.Tech CSE"}, {text: "Frontend Assignment"}],
};

const List = memo(WrappedListComponent);

export default List;
