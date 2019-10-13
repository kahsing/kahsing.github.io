---
layout: post
title:  "Sample Structure For React Native Application"
categories: [Mobile App]
tags: [React Native]
read: 10 min
---

Having thoughts of building mobile applications with skillset constraint on web development? Me too, and I had picked up React Native as my stepping stone to create nearly native mobile applications for Android and iOS.

<br />
#### Sample structure of a React Native Mobile Application -- Sunoar2u
***

![Structure Sunoar2u](/assets/img/2019_10_10/archi_sunoar2u.jpg)

***

#### The Backbone of A Mobile Application -- The Navigator

<a href="https://reactnavigation.org/">React Navigation</a> by <strong>The React Commnunity</strong>
```sh
"Routing and navigation for your React Native apps"
```
##### Sunoa2u is leveraging three types of Navigators :
1. SwitchNavigator
2. DrawerNavigator
2. TabNavigator
3. StackNavigator

<br />
#### The SwitchNavigator
***

* Used to cater to behviour of Authentication Flows
* The user opens the app.
* The app loads some authentication state from persistent storage (for example, AsyncStorage).
* When the state has loaded, the user is presented with either authentication screens or the main app, depending on whether valid authentication state was loaded.
* When the user signs out, we clear the authentication state and send them back to authentication screens.


<br />
#### The DrawerNavigator

<div class="row">
    <div class="col-sm-6">
        <ul>
            <li>Used to be the <strong>Outer Most Navigator</strong>, wrapping up Main Menus and Sub-Navigations.</li>
            <li>Giving a drawered-like feel of navigation options.</li>
            <li>Profile</li>
            <li>Order History</li>
            <li>Bonus Statement</li>
            <li>Enrolment</li>
            <li>E-Library</li>
        </ul>
    </div>
    <div class="col-sm-6">
        <p>
        <a class="btn btn-primary" data-toggle="collapse" href="#collapseExampleDrawer" role="button" aria-expanded="false" aria-controls="collapseExampleDrawer">
            <i class="glyphicon glyphicon-eye-open"></i>&emsp;See Sample Drawer 
        </a>
        </p>
        <div class="collapse" id="collapseExampleDrawer">
            <div class="card card-body">
                <img src="/assets/img/2019_10_10/drawer.png" />
            </div>
        </div>
    </div>
</div>

<br />
#### The TabNavigator

<div class="row">
    <div class="col-sm-6">
        <ul>
            <li>Used to be the formation of <strong>Main Top Menus</strong>.</li>
            <li>Home Page</li>
            <li>Personal Dashboards</li>
            <li>Shopping Flow</li>
            <li>Notification</li>
        </ul>
    </div>
    <div class="col-sm-6">    
        <p>
        <a class="btn btn-primary" data-toggle="collapse" href="#collapseExample" role="button" aria-expanded="false" aria-controls="collapseExample">
            <i class="glyphicon glyphicon-eye-open"></i>&emsp;See Sample BottomTab 
        </a>
        </p>
        <div class="collapse" id="collapseExample">
            <div class="card card-body">
                <img src="/assets/img/2019_10_10/dashboard.png" />
            </div>
        </div>
    </div>
</div>

<br />
#### The StackNavigator

<div class="row">
    <div class="col-sm-6">
        <ul>
            <li>Used to be the formation of <strong>Sub-Navigations</strong> for the above Top Menus.</li>
            <li>Giving screen stacking over each mobile app's view</li>
            <li>Deeply nesting under parent navigators (Drawer, Tabs, Stack)</li>
        </ul>
    </div>
    <div class="col-sm-6">    
        <p>
        <a class="btn btn-primary" data-toggle="collapse" href="#collapseExampleStack" role="button" aria-expanded="false" aria-controls="collapseExampleStack">
            <i class="glyphicon glyphicon-eye-open"></i>&emsp;See Sample Stack 
        </a>
        </p>
        <div class="collapse" id="collapseExampleStack">
            <div class="card card-body">
                <img src="/assets/img/2019_10_10/stack.png" />
            </div>
        </div>
    </div>
</div>

<br />

***

#### Notification -- Pushy
<p>
    <strong>Lightning-fast, highly-reliable push notification delivery</strong>
    <br />
    <ul>
        <li>Easy Integration with React Native</li>
        <li>Reference From :<a href="https://pushy.me/docs/additional-platforms/react-native"> this source. </a></li>
    </ul>
</p>

<br />

***

#### Payment -- Ewallet , Ipay88
<p>
    <strong>Lightning-fast, highly-reliable push notification delivery</strong>
    <br />
    <ul>
        <li>E-wallet</li>
            <ul>
                <li>Internally Self-managed E-account Payment Method of the company.</li>
            </ul>
        <li>IPAY88</li>
        <ul>
            <li>Commonly used payment mehthod. </li>
            <li>Cost Effective for SME.</li>
            <li>Reference From :<a href="https://pushy.me/docs/additional-platforms/react-native"> Official. </a></li>
            <li>React Native Integration :<a href="https://github.com/myussufz/react-native-ipay88-sdk"> Open-source. </a></li>
        </ul>
    </ul>
</p>

<br />

***

#### React-Native WebViews
<div class="row">
    <div class="col-sm-6">
        <ul>
            <li>Fast Delivery</li>
            <li>Easy Content Displaying.</li>
            <li>Cost Effective for graphical content.</li>
        </ul>
    </div>
    <div class="col-sm-6">    
        <p>
        <a class="btn btn-primary" data-toggle="collapse" href="#collapseExampleWebView" role="button" aria-expanded="false" aria-controls="collapseExampleWebView">
            <i class="glyphicon glyphicon-eye-open"></i>&emsp;See Sample WebView 
        </a>
        </p>
        <div class="collapse" id="collapseExampleWebView">
            <div class="card card-body">
                <img src="/assets/img/2019_10_10/webview.png" />
            </div>
        </div>
    </div>
</div>

<br />


***

#### E-library
<div class="row">
    <div class="col-sm-6">
        <ul>
            <li>Delivery of Multimedia Content</li>
            <li>Loading Video from Youtube Link.</li>
            <li>Media Player for video source.</li>
            <li>Displaying Images & Download them.</li>
            <li>Displaying Documents & Download them.</li>
        </ul>
    </div>
</div>

<br />

***

#### Detailed Design of the App

For more Detailed Design of the App, check it out from <a href="https://docs.google.com/document/d/1xVRQKi_Ha12eHAokyQ5JhlSPzz9CszPUlvMYSseel5c/edit?usp=sharing">The Link</a>

<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>