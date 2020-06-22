---
title: "SharePoint Framework: Passing data between React components"
date: "2020-03-30"
---

## Overview

React is one of the popular choices to develop SharePoint Framework solutions. Passing data between React components can be simple but sometimes it can get tricky. On a broader level, passing data between React components involves scenarios including passing of data from parent to child, from child to parent, and between siblings.

In this article, we will explore passing data between parent to child and vice-a-versa, which is a common scenario in SharePoint Framework development.

## Passing Data from Parent to Child

The SharePoint Framework solution involves multiple React components, each one with a unique responsibility. Parent component hosts multiple child components and passing data from Parent to child is a common need.

The simple graphical representation is as follows:

SPFx App
|–– Parent Component
    |–– Child Component 1
    |–– Child Component 2

The code representation for Parent Component is as follows:

class ParentComponent extends React.Component {

    state = { parentMessage : "Hello World" }
 
    render() {        
        return (
            <div>
                 <ChildComponent1/>            //no data to send             
                 <ChildComponent2 parentInput = {this.state.parentMessage} />
            </div>
        );
    }
}

In the above example, we want to pass the data from Parent to Child component which is inside Parent state. The data is being passed as properties on child components. The simplest option to pass the data from “Parent Component” to “Child Component” is by using props (React properties) defined on child components.

Please note state is one way to pass data to another component. However, it is not always necessary to use the state. You may pass the data in the form of variables or constants.

The implementation of the Child component will be as follows:

class Child2 extends React.Component {
    render() {        
        return (
            <div>
                The data from parent is: {this.props.dataFromParent}
            </div>
        );
    }
}

In the child component, we are defining a property to accept the data from the parent component.

## From Child to Parent Using Callbacks

In some scenarios, we might have to pass the data back from child to parent components. As an example, pass the processed data or any click event back to the parent.

Follow the below steps to implement the callback.

1. Define callback function in a parent component that accepts a parameter, which we want to access from a child.
2. Send the callback function as props to the child component.

The parent component will look like as below:

class Parent extends React.Component {
     state = { message: "" }

     callbackFunction = (childData) => {
          this.setState({message: childData})
     }

     render() {
        return (
            <div>
                 <Child1 parentCallback = {this.callbackFunction}/>
                 <p> {this.state.message} </p>
            </div>
     )}
}

In the child component, send the data back to parent using callback function.

class Child1 extends React.Component{
    sendData = () => {
         this.props.parentCallback("Hey Popsie, How’s it going?");
    }

    render() { 
        // Call function sendData to send data from child component to Parent component.
    }
};

Summary

Passing data between React components involves scenarios including passing of data from parent to child, from child to parent, and between siblings. In this article, we explored a common scenario in SharePoint Framework development, i.e. passing data between parent to child and vice-a-versa.

## Code Download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-react-parent-child-communication](https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-react-parent-child-communication)
