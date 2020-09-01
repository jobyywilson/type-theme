---
layout: post
title:  "Angular Components"
image: ''
date:   2020-05-02 11:30:00 +0800
tags:
- angular
description: ''
categories:
- Learn Angular 
---

<h2>Components</h2>

Angular is built upon components. The starting point of an Angular app is the bootstrapping of the initial parent component, much like the HTML DOM tree that starts with an HTML element and then branches down from there.

<img src="{{ site.baseurl }}/assets/img/angular/component-tree.JPG">

Angular runs on a component tree model. After Angular loads the first component with the bootstrap call, it then looks within that component's HTML view and sees if it has any nested components.

    <section>
      <component-1></component-1>
      <component-2></component-2>
    </section>


If so, Angular finds matches and runs the appropriate component code on those. This repeats for each component down the tree. A component in Angular is used to render a portion of HTML and provide functionality to that portion. It does this through a Component class in which you can define application logic for the component. 

    import { Component } from '@angular/core';

    @Component ({
       selector: 'my-app',
       template: ` <div>
          <h1>{{title}}</h1>
          <div>Learn Angular6 with examples</div>
       </div> `,
    })

    export class AppComponent {
       title: string = 'Welcome to Angular world';
    }



Each component gets configured with a selector, which tells Angular what markup element tag to associate the Component class logic with. 

