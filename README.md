Reminders
========

Demo : https://polymer-reminder.firebaseapp.com/reminders

"Extend Polymer Start Kit with a reminders section. It should have a list of reminders which user has entered as well as a way for users to add new reminders. The user should be able to enter a title for the reminder, more detailed description of the reminder as well as the due date . The user has to be able to save the reminder."

Solutions
-------------
> **Task Analyse:**

> - Understand Polymer and its Start Kit
> - Adding a reminder section to main contents.
> - Reminder features includes: title, detail, due date and list of exist reminders.

###Methods:

Compile reminder's features into a new component ```<my-reminder>``` and add it to the main content of the index.html, the app will render reminders section when it called.

> Adding reminder section to content:

>- Create a section selector `<a>` with attributes and add it to the drawer content of index.html

 > - Add a new `<section>` to main content of index.html.  Inside with ``<my-reminder>`` component.

 > - Set up navigation to reminders section at routing.html


----------

###Building  ```<my-reminder>```
Implement reminder's features by import different elements, binding their value to the declared properties and methods.

Solution:

User inputs => save to an object ```reminder:{ titel:"", details:"", due:""}``` => push objects into an array ```[reminder1, reminder2, reminder3, ..]``` =>  display the array in grid (reminders list)


Methods:

>-  create app/elements/my-reminders/my-reminder.html

 ```<my-reminder>```  imports following elements for its interface,

 - ```<paper-input>```  -  input title and detail input field.
 -  ```<paper-button>```-  reminder submit.
 - ```<vaadin-date-picker>``` - due date selector
 - ```<vaadin-grid>``` - display exit reminders

######Vaadin elements has some good UI components supports polymer, in this app ```<vaddin-date-picker>``` and `<vaadin-grid>` were chose to implement due date and reminders features. vaddin-grid instruction:  https://vaadin.com/elements/-/element/vaadin-grid

 - `<iron-localstorage>`  -  data saving.



Binding the user input's value to the properties of an object called reminder:

> Title:`<paper-input value="{{reminder.title}}">`
> Details:`<paper-input value="{{reminder.details}}">`
> Due : `<paper-input value="{{reminder.due}}">`

Declare the reminder object and an array called reminders in Polymer( )
```  
Polymer({
is:"my-reminder",
properties:{
	reminder: {
    type: Object,
    value: {}
    },
    reminders: {
    type: Array,
    value: []
    },
      ...


```
The reminder object will have three properties:
`{title:"", details:"", due:""}`



 Push the reminder object into reminders (array) when user click button.

```
<paper-button class="submit-button" on-tap="_addReminder">

Polymer({
	...
  _addReminder: function() {
      //push to the reminders array
      this.push('reminders', this.reminder);
      // empty the form
      this.reminder = {};
      //console.log(this.reminders)
    },
    ...
    })
```

the `console.log(this.reminders)`  within Polymer() should shows :
`[reminder,reminder,reminder,...]`

Using the properties of element `<vaadin-grid>` to list all the objects within reminders in grid form.

```
<vaadin-grid items="{{reminders}}">
      <table>
       <!-- Define the columns and their mapping to data properties. -->
       <col name="title">
       <col name="details">
       <col name="due">
       ...
       </vaadin-grid>
```

save the reminders data by using `<iron-localstorage>` :

 ```<iron-localstorage value="{{reminders}}" on-iron-localstorage-load-empty="initializeDefaultReminder"></iron-localstorage>```  

 initialises default if nothing has been stored
```
Polymer({
...
  initializeDefaultReminder: function(){
      this.reminders = [];
    },
    ...
})
```


Clean the reminder list by empty the reminders array.

```
<paper-button class="empty-button" on-tap="_cleanReminder">Clean Reminders</paper-button>
<script>
Polymer({
...
   _cleanReminder: function() {
      this.reminders=[];
    },   
...
})
</script>

```



#Tests

Polymer Starter Kit included Web Component Tester for unit testing, simply use ```wct``` tool will run tests in all the browsers that have installed.

Install globally
```npm install -g web-component-tester```

run it with
```
wct
```

Alternatively we can run the test through Polymer CLI also powered by Web Component Tester
```
 polymer test
```

Both of them will run any tests under ```test/``` .
