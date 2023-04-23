SteelEye Frontend

1. Explain what the simple List component does.

The Simple List component is a user interface (UI) component that displays a list of items in a vertical or horizontal format. It is a commonly used UI element in many web and mobile applications.

The Simple List component usually displays text or icons for each item, and it can be customized to include additional elements such as images, checkboxes, or buttons. The list can be static or dynamic, meaning that it can be populated with data from a database or a user input.

The Simple List component can also be used in combination with other UI components to create more complex interfaces, such as dropdown menus or navigation bars. Its simplicity and versatility make it a popular choice for many developers and designers.

2. What problems / warnings are there with code?

In the WrappedSingleListItem, the onClickHandler is not being invoked properly. Instead of being called when the list item is clicked, the onClickHandler is being called immediately when the component is rendered.
onClick={onClickHandler(index)}

Incorrect syntax of useState() hook. The useState() hook returns an array of two elements where the first element is the state value and the second element is a function to update the state value. The array elements need to be swapped.
const [setSelectedIndex, selectedIndex] = useState();

In the items.map() function, each child in a list should have a unique key prop. Also, the isSelected prop should be passed as a boolean, instead it is being passed as a selectedIndex state value. The correct code should be:
{items.map((item, index) => ( <SingleListItem onClickHandler={() => handleClick(index)} text={item.text} index={index} isSelected={selectedIndex} /> ))}

In the WrappedListComponent, the prop type for the items array is not defined correctly.
PropTypes.array(PropTypes.shapeOf({...}))

The items in WrappedListComponent.defaultProps cannot be null. They must have an array of elements.
WrappedListComponent.defaultProps = { items: null, };

3. Please fix, optimize, and/or modify the component as much as you think is necessary.

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
      onClick={()=> onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState();

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
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={index === selectedIndex}
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
  items: [{
    text: "Daksh Patidar 12019231",
  },
  {
    text: "SteelEye Frontend Assignment ",
  },
  {
    text: "SteelEye was established in October 2017 to reduce the complexity and cost of compliance and to enable financial firms globally to manage their regulatory obligations through a single platform.",
  },
  {
    text: "SteelEye is the first and only truly integrated surveillance solution. We empower you to focus on what matters, all from a single platform.",
  },
]
};

const List = memo(WrappedListComponent);

export default List;