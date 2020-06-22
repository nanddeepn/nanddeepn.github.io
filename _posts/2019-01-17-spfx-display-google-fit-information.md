---
title: "SharePoint Framework - Display Google Fit Information"
date: "2019-01-17"
---

## Overview

Google Fitness REST APIs are useful if you have fitness app and you want to integrate your data with google fit or if you just want to collect Fitness data and display some information to the users. The Google Fit REST APIs can be consumed in SharePoint Framework.

In this article, we will explore how we can use Google Fit REST APIs in SPFx webpart and develop web part which helps to display the key fitness information (activity time spent, distance travelled, calories burned, step count) from the Google fit data source. We will use React JS in this example. For this article, I am using SharePoint Framework version 1.7.1

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md react-google-fit

1. Navigate to above created directory.

cd react-google-fit

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-309.png)

- **Solution Name:** Hit enter to have default name (react-google-fit in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the current folder or create a subfolder for our solution.
    - Selected choice: Use the current folder
- **Deployment option:** We may choose to allow the tenant admin the choice of being able to deploy the solution to all sites immediately without running any feature deployment or adding apps in sites.
    - Selected choice: N (install on each site explicitly)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
    - Selected choice: WebPart
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: GoogleFitActivityViewer
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Display Google Fit Activities
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

NPM Packages Used

react-google-authorize ([https://www.npmjs.com/package/react-google-authorize](https://www.npmjs.com/package/react-google-authorize))

This npm package helps to authenticate and authorize to Google.

1. Open “\\src\\webparts\\googleFitActivityViewer\\components\\GoogleFitActivityViewer.tsx”
2. Include the package.

import { GoogleAuthorize } from 'react-google-authorize';

1. Use the GoogleAuthorize component in render method.

public render(): React.ReactElement<IGoogleFitActivityViewerProps> {  
    const responseGoogle = (response) => {  
    }  
  
    return (  
      <div className={styles.googleFitActivityViewer}>  
        <div className={styles.container}>  
          {  
            !this.state.isGoogleAuthenticated && this.state.accessToken == "" &&  
            <GoogleAuthorize  
              scope={'https://www.googleapis.com/auth/fitness.activity.read https://www.googleapis.com/auth/fitness.location.read'}  
              clientId={this.props.clientId}  
              onSuccess={responseGoogle}  
              onFailure={responseGoogle}  
            >  
              <span>Login with Google</span>  
            </GoogleAuthorize>  
          }  
        </div>  
      </div>  
    );  
}

We are including below scopes

- [https://www.googleapis.com/auth/fitness.activity.read](https://www.googleapis.com/auth/fitness.activity.read) - To read the fitness activities (calories burned, step count)
- [https://www.googleapis.com/auth/fitness.location.read](https://www.googleapis.com/auth/fitness.location.read) - To read activity time spent, distance travelled

## Define State

1. Create a new file IGoogleFitActivityViewerState.ts under “\\src\\webparts\\googleFitActivityViewer\\components\\” folder.

export interface IGoogleFitActivityViewerState {  
    isGoogleAuthenticated: boolean;  
    accessToken: string;  
    stepCount: number;  
    calories: number;  
    distance: number;  
    activityTime: number;  
}

1. Update our component “\\src\\webparts\\googleFitActivityViewer\\components\\ GoogleFitActivityViewer.tsx” to use the state.

import { IGoogleFitActivityViewerState } from './IGoogleFitActivityViewerState';  
   
export default class GoogleFitActivityViewer extends React.Component<IGoogleFitActivityViewerProps, IGoogleFitActivityViewerState> {  
   
  public constructor(props) {  
    super(props);  
  
    this.state = {  
      isGoogleAuthenticated: false,  
      accessToken: "",  
      stepCount: 0,  
      calories: 0,  
      distance: 0,  
      activityTime: 0  
    };  
  }  
}

Implement the Service

We will create a service to query Google Fit REST APIs.

**Interface to represent REST API data**

Let us define an interface to represent the data returned by REST APIs.

1. Create a folder “services” under “src” folder
2. Add file IFitnessActivity.ts

export interface IFitnessActivity {  
    dataSourceId: string;  
    maxEndTimeNs: string;  
    minStartTimeNs: string;  
    point: IFitnessPoint\[\];  
}  
  
export interface IFitnessPoint {  
    dataTypeName: string;  
    endTimeNanos: string;  
    modifiedTimeMillis: string;  
    value: IFitnessPointValue\[\];  
}  
  
export interface IFitnessPointValue {  
    intVal: number;  
    fpVal: number;  
}

**Implement Generic Interface**

1. Add file IDataService.ts under “\\src\\services” folder.

export interface IDataService {  
    getStepCount: (accessToken: string) => Promise<any>;  
    getCalories: (accessToken: string) => Promise<any>;  
    getDistance: (accessToken: string) => Promise<any>;  
    getActivityTime: (accessToken: string) => Promise<any>;  
}

**Implement Google Fit Interface**

1. Add file GoogleFitService.ts under “\\src\\services” folder implementing IDataService interface.

import { ServiceScope, ServiceKey } from "@microsoft/sp-core-library";  
import { IDataService } from './IDataService';  
import { HttpClient, HttpClientResponse, IHttpClientOptions } from '@microsoft/sp-http';  
import { PageContext } from '@microsoft/sp-page-context';  
import { IFitnessActivity, IFitnessPoint, IFitnessPointValue } from './IFitnessActivity';  
  
export class GoogleFitService implements IDataService {  
    public static readonly serviceKey: ServiceKey<IDataService> = ServiceKey.create<IDataService>('googleFit:data-service', GoogleFitService);  
    private \_httpClient: HttpClient;  
    private \_pageContext: PageContext;  
  
    constructor(serviceScope: ServiceScope) {  
        serviceScope.whenFinished(() => {  
            // Configure the required dependencies    
            this.\_httpClient = serviceScope.consume(HttpClient.serviceKey);  
            this.\_pageContext = serviceScope.consume(PageContext.serviceKey);  
        });  
    }  
  
    // Get step count from Google fit data source  
    public getStepCount(accessToken: string): Promise<number> {  
        return new Promise<number>((resolve: (itemId: number) => void, reject: (error: any) => void): void => {  
            this.getGoogleFitData('derived:com.google.step\_count.delta:com.google.android.gms:estimated\_steps', accessToken)  
                .then((fitnessData: IFitnessActivity): void => {  
                    var stepsCount: number = 0;  
                    var i: number = 0;  
                    var j: number = 0;  
  
                    // Calculate step count of each activity  
                    for (i = 0; i < fitnessData.point.length; i++) {  
                        for (j = 0; j < fitnessData.point\[i\].value.length; j++) {  
                            stepsCount += fitnessData.point\[i\].value\[j\].intVal;  
                        }  
                    }  
  
                    resolve(stepsCount);  
                });  
        });  
    }  
  
    // Get calories burned from Google fit data source  
    public getCalories(accessToken: string): Promise<number> {  
        return new Promise<number>((resolve: (itemId: number) => void, reject: (error: any) => void): void => {  
            this.getGoogleFitData('derived:com.google.calories.expended:com.google.android.gms:merge\_calories\_expended', accessToken)  
                .then((fitnessData: IFitnessActivity): void => {  
                    var calories: number = 0;  
                    var i: number = 0;  
                    var j: number = 0;  
  
                    // Calculate calories burned during each activity  
                    for (i = 0; i < fitnessData.point.length; i++) {  
                        for (j = 0; j < fitnessData.point\[i\].value.length; j++) {  
                            calories += fitnessData.point\[i\].value\[j\].fpVal;  
                        }  
                    }  
  
                    resolve(calories);  
                });  
        });  
    }  
  
    // Get distance travelled from Google fit data source  
    public getDistance(accessToken: string): Promise<number> {  
        return new Promise<number>((resolve: (itemId: number) => void, reject: (error: any) => void): void => {  
            this.getGoogleFitData('derived:com.google.distance.delta:com.google.android.gms:merge\_distance\_delta', accessToken)  
                .then((fitnessData: IFitnessActivity): void => {  
                    var distance: number = 0;  
                    var i: number = 0;  
                    var j: number = 0;  
  
                    // Calculate distance travelled during each activity  
                    for (i = 0; i < fitnessData.point.length; i++) {  
                        for (j = 0; j < fitnessData.point\[i\].value.length; j++) {  
                            distance += fitnessData.point\[i\].value\[j\].fpVal;  
                        }  
                    }  
  
                    resolve(distance / 1000);  
                });  
        });  
    }  
  
    // Get activity time from Google fit data source  
    public getActivityTime(accessToken: string): Promise<number> {  
        return new Promise<number>((resolve: (itemId: number) => void, reject: (error: any) => void): void => {  
            this.getGoogleFitData('derived:com.google.activity.segment:com.google.android.gms:merge\_activity\_segments', accessToken)  
                .then((fitnessData: IFitnessActivity): void => {  
                    var activityTime: number = 0;  
                    var i: number = 0;  
                    var j: number = 0;  
  
                    // Calculate activity time spent for each activity  
                    for (i = 0; i < fitnessData.point.length; i++) {  
                        for (j = 0; j < fitnessData.point\[i\].value.length; j++) {  
                            activityTime += fitnessData.point\[i\].value\[j\].intVal;  
                        }  
                    }  
  
                    resolve(activityTime);  
                });  
        });  
    }  
  
    // Get Google fit data by calling the REST API  
    private getGoogleFitData(activityScope: string, accessToken: string): Promise<IFitnessActivity> {  
        // Calculate start date, end date  
        var startTime: number = new Date().getTime();  
        var todayMidnight: Date = new Date();  
        todayMidnight.setHours(0, 0, 0, 0);  
        var endTime: number = todayMidnight.getTime();  
  
        const requestHeaders: Headers = new Headers();  
        requestHeaders.append("Content-type", "application/json");  
        requestHeaders.append("Cache-Control", "no-cache");  
  
        const postOptions: IHttpClientOptions = {  
            headers: requestHeaders  
        };  
  
        let sessionUrl: string = \`https://www.googleapis.com/fitness/v1/users/me/dataSources/\` + activityScope + \`/datasets/\` + startTime + \`000000-\` + endTime + \`000000?access\_token=\` + accessToken;  
        return new Promise<IFitnessActivity>((resolve: (itemId: IFitnessActivity) => void, reject: (error: any) => void): void => {  
            this.\_httpClient.get(sessionUrl, HttpClient.configurations.v1, postOptions)  
                .then((response: HttpClientResponse) => {  
                    response.json().then((responseJSON: IFitnessActivity) => {  
                        resolve(responseJSON);  
                    });  
                });  
        });  
    }  
}

Code the WebPart

1. Open webpart GoogleFitActivityViewer.tsx under “\\src\\webparts\\googleFitActivityViewer\\components\\” folder.
2. Implement render method.

public render(): React.ReactElement<IGoogleFitActivityViewerProps> {  
  const responseGoogle = (response) => {  
    this.setState(() => {  
      return {  
        ...this.state,  
        isGoogleAuthenticated: true,  
        accessToken: response.access\_token  
      };  
    });  
  
    this.readStepCount(this.state.accessToken);  
    this.readCalories(this.state.accessToken);  
    this.readDistance(this.state.accessToken);  
    this.readActivityTime(this.state.accessToken);  
  };  
  
  const formatNumber = (num) => parseFloat(num.toFixed(2)).toLocaleString().replace(/\\.(\[0-9\])$/, ".$10");  
  
  return (  
    <div className={styles.googleFitActivityViewer}>  
      <div className={styles.container}>  
        {  
          !this.state.isGoogleAuthenticated && this.state.accessToken == "" &&  
          <GoogleAuthorize  
            scope={'https://www.googleapis.com/auth/fitness.activity.read https://www.googleapis.com/auth/fitness.location.read'}  
            clientId={this.props.clientId}  
            onSuccess={responseGoogle}  
            onFailure={responseGoogle}  
          >  
            <span>Login with Google</span>  
          </GoogleAuthorize>  
        }  
  
        {  
          this.state.isGoogleAuthenticated &&  
          <div>  
            <div className={styles.msTable}>  
              <div className={styles.msTableRowHeader}>  
                <span className={styles.msTableCell}>  
                  Today, {new Date().toDateString()}  
                </span>  
              </div>  
            </div>  
  
            <div className={styles.msTable}>  
              <div className={styles.msTableRow}>  
                <span className={styles.msTableCell}>  
                  <Icon iconName="Clock" className="ms-IconExample" />  
                </span>  
                <span className={styles.msTableCell}>  
                  <b>{formatNumber(this.state.activityTime)}</b> min  
              </span>  
              </div>  
  
              <div className={styles.msTableRow}>  
                <span className={styles.msTableCell}>  
                  <Icon iconName="POI" className="ms-IconExample" />  
                </span>  
                <span className={styles.msTableCell}>  
                  <b>{formatNumber(this.state.distance)}</b> km  
              </span>  
              </div>  
  
              <div className={styles.msTableRow}>  
                <span className={styles.msTableCell}>  
                  <Icon iconName="CaloriesAdd" className="ms-IconExample" />  
                </span>  
                <span className={styles.msTableCell}>  
                  <b>{formatNumber(this.state.calories)}</b> calories  
              </span>  
              </div>  
  
              <div className={styles.msTableRow}>  
                <span className={styles.msTableCell}>  
                  <Icon iconName="Running" className="ms-IconExample" />  
                </span>  
                <span className={styles.msTableCell}>  
                  <b>{formatNumber(this.state.stepCount)}</b> steps  
              </span>  
              </div>  
            </div>  
          </div>  
        }  
      </div>  
    </div>  
  );  
}

1. Implement the helper methods and set the states from it.

private readStepCount(accessToken: string): void {  
  let serviceScope: ServiceScope = this.props.serviceScope;  
  this.dataCenterServiceInstance = serviceScope.consume(GoogleFitService.serviceKey);  
  
  this.dataCenterServiceInstance.getStepCount(accessToken).then((stepCount: number) => {  
    this.setState(() => {  
      return {  
        ...this.state,  
        stepCount: stepCount  
      };  
    });  
  });  
}  
  
private readCalories(accessToken: string): void {  
  let serviceScope: ServiceScope = this.props.serviceScope;  
  this.dataCenterServiceInstance = serviceScope.consume(GoogleFitService.serviceKey);  
  
  this.dataCenterServiceInstance.getCalories(accessToken).then((calories: number) => {  
    this.setState(() => {  
      return {  
        ...this.state,  
        calories: calories  
      };  
    });  
  });  
}  
  
private readDistance(accessToken: string): void {  
  let serviceScope: ServiceScope = this.props.serviceScope;  
  this.dataCenterServiceInstance = serviceScope.consume(GoogleFitService.serviceKey);  
  
  this.dataCenterServiceInstance.getDistance(accessToken).then((distance: number) => {  
    this.setState(() => {  
      return {  
        ...this.state,  
        distance: distance  
      };  
    });  
  });  
}  
  
private readActivityTime(accessToken: string): void {  
  let serviceScope: ServiceScope = this.props.serviceScope;  
  this.dataCenterServiceInstance = serviceScope.consume(GoogleFitService.serviceKey);  
  
  this.dataCenterServiceInstance.getActivityTime(accessToken).then((activityTime: number) => {  
    this.setState(() => {  
      return {  
        ...this.state,  
        activityTime: activityTime  
      };  
    });  
  });  
}

Add Authorized JavaScript Origins

Please refer my previous article ([https://nanddeepnachanblogs.com/2019/01/google-fit-rest-api-overview/](https://nanddeepnachanblogs.com/2019/01/google-fit-rest-api-overview/)) to generate OAuth 2.0 client ID.

1. Open Google Developer Dashboard from here ([https://console.developers.google.com/apis/dashboard](https://console.developers.google.com/apis/dashboard))
2. Select the project created by following instructions from previous article.
3. From left navigation click Credentials
4. Click listed web client

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-310.png)

1. Under Authorized JavaScript origins, add SharePoint Online site url (e.g. https://contoso.sharepoint.com ) or https://localhost:4321 , if you are using SharePoint local workbench.
2. Under Authorized redirect URI, add https://localhost:4321/auth/google/callback, if you are using SharePoint local workbench.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-311.png)

1. Click Save.

## Configure the Web Part to use

1. Add "Google Fit Activity Viewer" web part on SharePoint page.
2. Edit the web part.
3. Add above generated OAuth 2.0 client ID to "ClientId Field" web part property.
4. Save the changes.

## Summary

Google Fit REST APIs can be consumed in SharePoint Framework web part to display the key fitness information (activity time spent, distance travelled, calories burned, step count) from the Google fit data source. Npm package (react-google-authorize) helps to authenticate and authorize to the scopes in Google.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-display-google-fit-information/).
