## Lightning Web Components
***
### Introduction 
- For building components - Uses the core concepts od web standards
- Benefits
  - LightWeight Framework
  - Better Performance
  - No need to learn different framework to develop application in salesforce
  - Interoperability with lightning Aura components
  - Better testing using Jest and Better security
- Coexistence and interoperability
 Aura components and LWC can coexist and interoperate and they can share the same high level services:
   - Can coexist on same page
   - Aura components can include LWC but not other way around
   - Share the same base LC, Base LCs were already implemented as LWCs
   - ACs and LWCs share underlying services(Lightning data service, User interface API, etc.)
-Lightning Experience - New UI for salesforce installations - upgrade from classic view, bringingmodern appearance and functionality to the salesforce platform.
***
### SF Developers Experience(DX)
- Set of tools - streamlines the entire development life cycle
- Improves team development and collaboration, facilitates automatedtesting and continuous integration and makes release cycle more efficient and agile
  - VS code
  - SF CLI
  - SF Extension Pack
***
###  My Domain
 - custom domain - more secure, Some SF features requires it
### Dev Hub
 - Enable Dev Hub to 
   1. Create and Manage orgs from the command line
   2. View Information about Scratch orgs
   3. Link Namespace orgs
 - Setup => Dev Hub
***
### Command to create a project
- Command: **sfdx force:project:create -n "project name"** (-n => new project)
- **Project Creation** - view => command palatte => SFDX => create Project with Manifest => generates package.xml along with project creation
- Three options 
  - Standard (Default)
  - Empty
  - Analytics
- To link dev hub of your account to VS code project
  - **Authorization of org** - view => Command Palatte => auth => two options => Authorize an Org, Authorize a Dev Hub => select Authorize a Dev Hub
  - Command: **sfdx force:auth:web:login -a SalesforceBlitzscale -d** (-a => alias, -d => default)
***
### Dev Hub vs Scratch org
1. Dev Hub - Main SF org that you will use to create and manage your scratch orgs.
2. Scratch Org - Source-Driven and disposable deployment of SF code and metadata. Scratch Orgs are driven by source, sandboxes are copies of production
#####Note:
- Scratch orgs do not replace sandboxes
***
### Command to create a Scratch Org(Expires in 30 days)
1. View => Command Palatte => Create a default scratch org
2. Command: 
  - To create a scratch org with data => add  ("hasSampleData": true) property to project-scratch-def.json
  - Path to scratch json file => config => project-scratch-def.json
  - Enter: **sfdx force:org:create -a lwcScratchOrgOne -d 30 -f config/project-scratch-def.json -s**(-s set it to default username,-d no. of days scratch org alive 1 day min and 30 day max, -f file location of project scratch definition json)
  - Need to specify one parameter => if we create it without this => Scratch org will not have any data
3. To open scratch org => Can open through the icon in status bar (or) Command: **sfdx force:org:open
***
### Master Command
- To look into all commands => **sfdx commands**
### Comment
- ctrl+/(backslash) or <!-- hello -->
***
### Hint
- Anytime you encounter error "no org configuration for name" then run the following command
- **sfdx force:config:set defaultdevhubusername=<the DX DexHub username>
- Eg: sfdx force:config:set defaultdevhubusername= salesforce@dev.com
***
---
### Fundamentals of LWC
####Creating components
- **Component Naming Convention**
  - Begin - Lowercase
  - Only alphanumric or underscore characters
  - Unique in namespace
  - Can't (include whitespace, end with an underscore, contain two consecutive underscores, contin a hyphen(dash))
- Two ways
  - Command: **sfdx force:lightning:component:create --type lwc -n helloWorld
  - Command Palatte => Type create lightning web, Component => Hit Enter => Enter desired filename => hit Enter => Again hit enter to choose default path(lwc)
- Component folder strucute
  - Main - (hW.html, hW.js, hW.js-meta.xml), Other - (hW.css, hW.svg, moreSharedCode.js, __test__/hW.test.js)
- Naming Conventions for LWC
  1. CamelCase - Eg: helloWorld (Component Name)
  2. PascalCase - Eg: HelloWorld (Component Class Name)
  3. Kebab-case - Eg: hello-world (Component reference and HTML attribute Name)
- App Creation and Component Deployment ( App Creation => Page Creation => Component Deployment => Component Assignment )
  1. Go to stratch org => setup => App manager, then also create page using Setup => Lightning App Builder
  2. Component Deployment - in VS code => in .js-meta.xml => Change isExposed to true and add target page to it, then use command: sfdx force:source:push
- Local Properties and Data Binding
  1. Local Properties - variables(undefined,object,string,boolean,array) inside class(only accessed by particular clas in which it is declared)
  2. Data Binding in template - Synchronization b/w controller(JS) and the template(HTML) (Eg: JS => fullname ="poorni", HTML => My name is {fullname} )
  3. Notes - In templates => can access property value directly if it's primitive or object, Dot notation => to acess object, doesn't allow exp => name[2] or {2+2}, property in {} must be a valid JS identifier or member exp({name} or{user.name}, Avoid adding spaces around property => { name }
- Methods and Two way data dinding
  1. Can call method from HTML or from class itself 
  2. Can add styling in two ways - salesforce component library, Lightning design system
- @track properties
  1. When a field contains object or array, there's a limit to the depth of changes that are tracked.
  2. To tell the framework to observe changes to the properties of an object or element of array, decorate the field with @track
  3. To use, import {track} from 'lwc' and wrap the object or array with @track in js file 
- Normal property vs @track property
  1. Without using @track, the framework observes changes that assign a new value to the field/property.
  2. If the new value is not === to the previous value, the component re-renders.
- Getters
  1. Getter is always a method appen with **get** keyword, always returns something
  2. Getter is always used to show manipulated data or updated data on the screen
  3. When to use => HTML page will not allow any expression or array(eg: {num1*num2} or {user[0]}, so append **get** with the method and insert expressions in JS file, Everytime data changes gets updated to HTML page.
- Conditional Rendering
  - Directives
     - Special HTML attributes, LWC programming model => few custom directives => let you manipulate DOM using markup
  -LWC => Two directives for Conditional rendering
     - if:true => to render DOM elements in template, if the expression is true. 
     - if:false => False
     - Exp can be JS identifier(property), JS dot notation that accesses a property from an object(user.fullname), can't use ternary operator inside the expression, to compute the value of expression => use getter in JS class.
     - In JS, Falsy values => If the property has => X = (0, false, undefined, null, "") => if:true will always be false
-Template Looping
  1. Template loop will always works in the array
  2. Key - string attribute => need to inculde to the first element inside the template when creating list of elements, helps LWC to identify which item have changed, added or removed. To pick a key => use string to uniquely identify a list item among its siblings. Key must be number or string(can't be object and can't use value of index as a key)
  3. Looping types
     1. for:Each
**syntax**
```
<template for:each={array} for:item="currentItem" for:Index="index">
-----repeatable template-----
</template>
```
     2. Iterator - to apply special behaviour to first or last item in the list. Can access value(iterationName.value.propertyName), index(iterationName.index), first(iterationName.first), last(iterationName.last)
**syntax**
```
<template iterator:iterationName={array}>
-----repeatable template-----
</template>
```
***
### Component Composition
- Composition - Adding component in the body of another component(enables - enables you to build complex components from simpler building-block components)
- To refer child components name in parent components
  1. childComponent - <c-child-component></c-child-component>
  2. childComponentDemo - <c-child-component-demo></c-child-component-demo>
  3. sampleDemoLWC - <c-sample-demo-l-w-c></c-sample-demo-l-w-c>
  4. Camelcase - kebabcase
- Shadow DOM 
   1. Brings encapsulation concept of HTML which enables you to link hidden separated DOM to an element
   2. Benefits - DOM queries, event propagation and CSS rules cannot go to the other side of the shadow boundary, thus creating encapsulation
Eg:
```
<!DOCTYPE html>
<html>
    <head>
        <title>Shadow DOM</title>
        <style>
            p{
                color:red;
            }
        </style>
    </head>
    <body>
       <div>
           <p>Hello</p>
       </div>
       <div id="host"></div>
       <button onclick="fetchPtag()">Click me</button>

       <script>
           const elem = document.querySelector('#host')
           const shadowRoot = elem.attachShadow({mode:'open'})
           shadowRoot.innerHTML = '<p>I am shadow DOM</p>'

           function fetchPtag(){
               let tags = document.querySelectorAll('p')
               console.log(tags)
           }
       </script>
    </body>
</html>
```
- Accessing Elements in components
  1. Elements rendered by component => Use template property
     - this.template.querySelector(selector);
     - this.template.querySelectorAll(selector);
     - element.template.querySelectorAll(selector);
  2. Note - Don't use ID selector with queryselector
  3. lwc:dom="manual" => add this directive to native HTML element to attach an HTML element as a child
***
### Styling in LWC
- Inline Styling - High priority
- External CSS - elements, Classes, Pseudo(eg: hover)
- Using Lightning Design System 
  - https://www.lightningdesignsystem.com/
- SLDS Design Token
  - Format - lightning => Global, Internal
  - To use => eg: color:var(--lwc-brandAccessible)
- Shared CSS in LWC
  - To use common css styling to different components => create a common component and import it to other components using => @import 'c/cssLibrary(component name)';
- Dynamic Styling
- Third-party CSS library
- Applying CSS across shadow DOM
***
### Component lifecycle Hooks
- Callback method triggered at a specific phase of component instance lifecycle
  1. Mounting Phase => constructor(), connectedCallback(), render(), renderCallback()
  2. Unmounting Phase => disconnectedCallback()
  3. Error Phase => errorCallback()
- Render - technically not a lifecycle hook, but it's a protective method on the lightning element class
#### Mounting Phase(Creation phase)
LWC Creation and Render Life cycle
- Parent Component
Com Lifecycle Start => **constructor** => property to update => com inserted into DOM => **connectedCallback** called => com rendered => Any child com => goes to child com and same procedure takes place => **renderedCallback** called => com lifecycle End
##### Constructor()
- Invoked, when component instance is created
- First call - super() inside this
- At this pt, Component properties won't be ready yet
- To access the host element => use this.template
- Flow => Parent to child component
- Can't access child elements in component body because they don't exist yet
- Don't add attributes to host element in constructor
##### connectedCallback Method
- Called when element is inserted into a document
- Hook flow => parent to child
- Can't access child elements in component body because they don't exist yet
- To access the host element => use this.template
- Use it to - perform initialization tasks => fetch data, set up caches, or listen for events(publish-subscribe events)
- Do not use this method - to change the state of component => loading values or setting properties. Use getters and setters instead
##### renderedCallback Method
- Fires when component rendering is done
- Can fire more than once
- Flows => child to parent
- When a component re-renders, all the expressions used in the template are re-evaluated
- Do not use this method - to change the state or update property of the component. Don't update a wire adapter configuration object property in this method, as it can result in an infinite loop.
#### Unmounting Phase(Removing Phase)
- Fired when component removed from the DOM
- Flow => Parent to child
- Specific to LWC, It isn't from the HTML custom elements specification
#### errorCallback(err,stack) Method
- Called when a descendant component throws an error in one of its callback
- Error argument => JS native error object, Stack argument => String
- Specific to LWC, It isn't from the HTML custom elements specification
#### render Method
- Tells the component = > which template to load based on some conditions => Always return the template reference
- technically not a lifecycle hook, but it's a protected method on the lightning element class
- Call to update the UI(may be called before or after the conncetedCallback()
- Rare usage. Main use is to conditionally render a template
- When to prefer multiple template over if:true/if:false
  - when there is small template to hide/show
  - to break down your component into smallest unit
  - In order to rendera component with more than one look and feel
  - When we have two design in same component => not to mix it in one html file
***
### Component Communication
#### Parent to child
- Parent to child through primitive(string,number,boolean), Non-primitive(array,object,array of object), passing data on action at parent, calling child method from parent
- @api decorator - to make field/property or method public, in order to expose the property
- An owner component that uses the component in its HTML markup can access its public properties via HTML attribute
- Public properties are reactive in nature and if the value changes => template re-renders
#### Child to parent
- Child to parent through Simple Event, Event with data, Event Bubbling
- To communicate with parent => Need custom events

##### Create and Dispatch Events
- CustomEvent syntax(By def => bubbles and composed => false)
```
new CustomEvent('event name',{bubbles:false, composed:false, details:'data to be passed'}) 
```
- dispatchEvent syntax(triggers custom event)
```
EventTarget.dispatchEvent(eventcreated)
```
Eg:
```
this.dispatchEvent(new CustomEvent('event name',{bubbles:false, composed:false, details:'data to be passed'}))
```
- Two event propagation
  - bubbles - Boolean value => event bubbles up through the DOM or not
  - composed - Boolean value => event can pass through the shadow boundary or not
#### Sibling component communication using PubSub
- PubSub - Publishing and subscribing
- Two ways => B/W independent components
  - PubSub
  - LMS
Note: If LMS => not serve your purpose, Use this(Because, it's an old technique)
- Communication across VF pages, Aura and LWC using Lightning Messaging Service(LMS)
***
### Setter Method
- To modify the data coming from parent component
- If object is passed as data to server, to mutate the object we have to create a shallow copy
### Passing Markup into Slots
- Slots - To pass HTML markup into another component
- <slot></slot> markup - to catch HTML passed by parent component
- Can't pass Aura component into a slot
- Two types of Slots
   - Named Slots - name attribute <slot name="user"></slot>
   - Unnamed Slots 
### CSS Behaviour in Parent Child Component
- Parent CSS can't reach into child component
- Parent component CSS can style a host element (<c-css-child>)
- Child Component CSS => can reach up and style its own host element
- Can style element and pass into slot from parent only














