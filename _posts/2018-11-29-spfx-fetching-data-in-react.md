---
title: "SharePoint Framework - Fetching Data in React"
date: "2018-11-29"
---

## Overview

React JS is commonly used to develop solutions in SharePoint Framework. It is a common practice to fetch the data from SharePoint site, Office 365, MS Graph API, or from any external service and display it in SharePoint framework web part. However there is always a confusion on which React method is the best place to fetch the data.

In this article, we will explore all possible options to fetch data from React component and pros cons of each option.

## Render Method of React Component

Each React component has Render method. The obvious thought is to have fetch mechanism inside Render method. However, it is not a good place to fetch the data the reason being that it causes the state change. Also, it is not advisable to perform any asynchronous calling in Render method.

The commonly used options are componentWillMount and componentDidMount.

## componentWillMount

This method is called right before React component’s first render occurrence. Just by reading this statement, it is tempting to have the fetching mechanism inside componentWillMount. But wait! This place might not be ideal for fetching the data as asynchronous calls to fetch the data might not return before Render method executes. This means, there are high chances that component will render empty data for at least once.

We neither can pause Render method until componentWillMount finishes, nor we can return a promise from componentWillMount. To handle this situation gracefully, we can setup the component initial state.

Let us take an example of fetching data from service.

class Employee extends Component {  
  componentWillMount() {  
    dataService.get('/mydata').then(result => {  
      this.setState({items: result.data});  
    });  
  }  
  
  render() {  
    return (  
      <ul>  
        {this.state.items.map(item =>  
          <li key={item.id}>{item.name}</li>  
        )}  
      </ul>  
    );  
  }  
}

In this example, componentWillMount is calling the dataService asynchronously which will result empty data for first occurrence.

To handle this situation, we can set the initial state in the constructor.

constructor(props) {  
    super(props);  
  
     this.state = {  
        items: \[\]  
    };  
}

We can also check for empty data in Render method.

render() {  
  return (  
    <ul>  
      {this.state && this.state.items && this.state.items.map(item =>  
        <li key={item.id}>{item.name}</li>  
      )}  
    </ul>  
  );  
}

componentWillMount is now deprecated, so it is not good idea of still keep using this method.

## componentDidMount

React component gets rendered once before calling componentDidMount. It is ideal to use this method to fetch the data for below reasons:

1. Data will not be loaded until after initial render. This will make us compulsory to setup the initial state to avoid undefined state errors.
2. In case app needs server rendering, componentWillMount will get called twice (one time on server and on client). Having data fetching mechanism in componentDidMount will ensure that data is fetched only once on client.

componentDidMount will always be called on client after first render when it has received the data. Try to avoid any kind of state change in componentDidMount, because it will cause re-rendering of the component and which in turn will call componentDidMount which will again call Render method and so on and on. This will cause an infinite loop.

componentDidMount can be used to setup long running processes that can fetch data from SharePoint or any external service on a periodic basis.

componentDidMount() {  
  this.interval = setInterval(this.fetchMyDaya, 3600000);  
}

Summary

While developing SharePoint Framework webpart with React, it is recommended to use componentDidMount over any other place. Try avoid using componentWillMount as it is deprecated. Render method is not a good place for fetching the data as it causes the state change.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-fetching-data-in-react/).
