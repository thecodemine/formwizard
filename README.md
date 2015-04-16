# FormWizard

FormWizard is a jQuery plugin which turns a regular HTML form into a wizard with very little effort.

The plugin supports AJAX form submission, form validation and browser back and forward buttons, all through integration with the following jQuery plugins:

* jQuery form
* jQuery validation
* [BBQ plugin](https://github.com/cowboy/jquery-bbq)

## Usage

Be sure to have the correct includes obviously - the plugin has been tested with the following versions, but should work with much newer ones as well.

* jquery-1.4.2.min.js or jquery-1.4.4.min.js
* jquery.form.js
* jquery.validate.js
* bbq.js
* jquery-ui-1.8.5.custom.min.js
* jquery.form.wizard-{version}

Example usage. This is a very simple setup.

```javascript
$(function() {
    $("#demoForm").formwizard({
        formPluginEnabled: true,
        validationEnabled: true,
        focusFirstInput : true,
        formOptions : {
            success: function(data) {
                $("#status").fadeTo(500,1,function(){
                    $(this).html("You are now registered!").fadeTo(5000, 0);
                })
            },
            beforeSubmit: function(data){
                $("#data").html("data sent to the server: " + $.param(data));
            },
            dataType: 'json',
            resetForm: true
        }
    });
});
```

# License
Licensed under the MIT license: http://www.opensource.org/licenses/mit-license.php

# Development
This plugin is no longer maintained.

# Options
* **historyEnabled** : boolean (Default value false)
 *  true enables the BBQ plugin. Enables navigation of the wizard using the browser's back and forward buttons
* **validationEnabled** : boolean (Default value false)
 * true enables the validation plugin
* **validationOptions** : Object (Default value undefined)
 * Holds options for the validation plugin. See http://jqueryvalidation.org/validate for specific options.
 * Requires: validationEnabled
* **formPluginEnabled** : boolean (Default value false)
 * true enables the form plugin. Makes sure that the plugin is posted via AJAX. Set to false if you want to post the form without using AJAX.
* **formOptions** : Object (Default value { reset: true, success: function(data) { alert("success"); })
 * Holds options for the form plugin. See form plugin documentation (http://jquery.malsup.com/form/) for specific options.
* **linkClass** : String (Default value ".link")
 * Selector which specifies the CSS-class of inputs used as links in the wizard
* **submitStepClass** : String (Default value "submit_step")
 * Specifies the CSS-class of the steps where the form should be submitted.
* **back** : String (Default value ":reset")
 * Specifies the elements used as back buttons
* **next** : String (Default value ":submit")
 * Specifies the elements used as next buttons
* **textSubmit** : String (Default value "Submit")
 * The text of the next button on submit steps
* **textNext** : String (Default value "Next")
 * The text of the next button on non-submit steps
* **textback** : String (Default value "Back")
 * The text of the back button.
* **remoteAjax** : Object (Default value undefined)
 * Object holding options for AJAX calls done between steps
* **inAnimation** : Object (Default value {opacity: 'show'})
 * Specifies the animation done during the in-transition between steps
* **outAnimation** : Object (Default value {opacity: 'hide'})
 * Specifies the animation done during the out-transition between steps
* **inDuration** : Number (Default value 400)
 * Specifies the duration of the in-animation between steps
* **outDuration** : Number (Default value 400)
 * Specifies the duration of the out-animation between steps
* **easing** : String (Default value "swing")
 * Specifies the easing used during the transition animations between steps. See jQuery Easing Plugin documentation (http://gsgd.co.uk/sandbox/jquery/easing/) for more information on easings.
* **focusFirstInput** : boolean (Default value false)
 *  Specifies whether the first input field on each step should be focused.
* **disableInputFields** : boolean (Default value false)
 *  Specifies whether the input fields in the form should be disabled during the initialization of the plugin. The disabling of inputs may be needed to be done in HTML if the number of input fields are very large, if this is needed - set this flag to false.
* **disableUIStyles** : boolean (Default value false)
 *  Specifies whether the wizard should use jquery UI styles or not.

# Events
* **after_remote_ajax**
 * This event is triggered after a remote ajax call has been done.
 * Returns: currentStep : String (id of the current step)
 * Example: 
        $(function(){
             // bind a callback to the after_remote_ajax event
            $("#demoForm").bind("after_remote_ajax", function(event, data){
                 alert(data.currenStep);
            });
        });
* **before_remote_ajax**
 * This event is triggered before a remote ajax call is done.
 * Returns: currentStep : String (id of the current step)
 * Example: 
        $(function(){
            // bind a callback to the before_remote_ajax event
            $("#demoForm").bind("before_remote_ajax", function(event, data){
                alert(data.currenStep);
            });
        });
* **before_step_shown**
 * This event is triggered just before a step is shown. It is triggered for both backwards and forwards navigation.
 * The data returned in the data variable is as follows (see the **state** method for further explanation of the properties):
   * isBackNavigation - boolean - true if the wizard navigated back and false otherwise
   * settings - Object - the current settings of the wizard (see Options)
   * activatedSteps - Array - contains the activated steps of the wizard
   * isLastStep - boolean - true if the current step is a submit step
   * isFirstStep - boolean - true if the first step is the current step
   * previousStep - String - the id of the previous step
   * currentStep - String - the id of the current step
   * backButton - Selector - contains the backButton
   * nextButton - Selector - contains the nextButton
   * steps - Selector - contains the steps of the wizard
   * firstStep - String - the id of the first step
 * Example: 
        $(function(){
            // bind a callback to the before_step_shown event
            $("#demoForm").bind("before_step_shown", function(event, data){
                alert(data.isBackNavigation);
            });
        });
* **step_shown**
 * This event is triggered when a step is shown. It's triggered for both back and forward navigation.
 * The data returned in the data variable is as follows (see the **state** method for further explanation of the properties):
   * isBackNavigation - boolean - true if the wizard navigated back and false otherwise
   * settings - Object - the current settings of the wizard (see Options)
   * activatedSteps - Array - contains the activated steps of the wizard
   * isLastStep - boolean - true if the current step is a submit step
   * isFirstStep - boolean - true if the first step is the current step
   * previousStep - String - the id of the previous step
   * currentStep - String - the id of the current step
   * backButton - Selector - contains the backButton
   * nextButton - Selector - contains the nextButton
   * steps - Selector - contains the steps of the wizard
   * firstStep - String - the id of the first step
 * Example: 
        $(function(){
            // bind a callback to the step_shown event
            $("#demoForm").bind("step_shown", function(event, data){
                alert(data.isBackNavigation);
            });
        });

# Methods
* **before_step_shown**
 * This event is triggered just before a step is shown. It is triggered for both backwards and forwards navigation.
 * This method returns the state of the wizard. The data returned is the following::
   * settings - Object - the current settings of the wizard (see Options)
   * activatedSteps - Array - contains the activated steps of the wizard
   * isLastStep - boolean - true if the current step is a submit step
   * isFirstStep - boolean - true if the first step is the current step
   * previousStep - String - the id of the previous step
   * currentStep - String - the id of the current step
   * backButton - Selector - contains the backButton
   * nextButton - Selector - contains the nextButton
   * steps - Selector - contains the steps of the wizard
   * firstStep - String - the id of the first step
 * Example method call:
        $("#demoForm").formwizard("state");
* Example data:
        {
            "settings" : {
                historyEnabled  : false,
                validationEnabled : false,
                validationOptions : undefined,
                formPluginEnabled : false,
                linkClass   : ".link",
                submitStepClass : "submit_step",
                back : ":reset",
                next : ":submit",
                textSubmit : 'Submit',
                textNext : 'Next',
                textBack : 'Back',
                remoteAjax : undefined, 
                inAnimation : {opacity: 'show'},
                outAnimation: {opacity: 'hide'},
                inDuration : 400,
                outDuration: 400,
                easing: 'swing',
                focusFirstInput : false,
                disableInputFields : true,
                formOptions : { reset: true, 
                        success: function(data) { alert("success"); }   
            },
            "activatedSteps" : ['firstStep','secondStep','thirdStep'],
            "isLastStep" : true,
            "isFirstStep" : false,
            "previousStep" : 'secondStep',
            "currentStep" : 'thirdStep',
            "backButton" : Object,
            "nextButton" : Object,
            "steps" : Object,
            "firstStep" : 'firstStep'
        }
* **show**
 * This method takes care of showing a specified step in the wizard.
 * Parameters:
    * id : String (the element id of the step to show)
 * Example method call:
        $("#demoForm").formwizard("show","firstStep");
* **reset**
 * This method takes care of resetting the wizard.
 * Example method call:
        $("#demoForm").formwizard("reset");
* **destroy**
 * This method takes care of removing the wizard functionality from the form.
 * Example method call:
        $("#demoForm").formwizard("destroy");
* **back**
 * This method triggers the wizard to move back.
 * Example method call:
        $("#demoForm").formwizard("back");
* **next**
 * This method triggers the wizard to move to the next step.
 * Example method call:
        $("#demoForm").formwizard("next");
* **update_steps**
 * This method triggers the wizard to check the form for available steps, and cache them.
 * Example method call:
        $("#demoForm").formwizard("update_steps");

#Theming
The classes **ui-formwizard**, **ui-formwizard-content**, **ui-wizard-content** and **ui-formwizard-button**  (see example example below), are set by the plugin to make it possible to add some additional styling - e.g. to the input fields or buttons.

               <form id="demoForm" method="post" action="json.html" class="bbq ui-helper-reset ui-formwizard ui-widget ui-widget-content ui-helper-reset ui-corner-all">
          <div id="fieldWrapper">
          <span style="display: inline;" class="step ui-formwizard-content ui-helper-reset ui-corner-all" id="first">
            <span>First step - Name</span><br>
            <label for="firstname">First name</label><br>
            <input class="ui-wizard-content ui-helper-reset ui-state-default" name="firstname" id="firstname"><br>
            <label for="surname">Surname</label><br>
            <input class="ui-wizard-content ui-helper-reset ui-state-default" name="surname" id="surname"><br>
          </span>
          <span style="display: none;" id="confirmation" class="step ui-formwizard-content ui-helper-reset ui-corner-all">
          <span>Last step - Username</span><br>
            <label for="username">User name</label><br>
            <input class="ui-wizard-content ui-helper-reset ui-state-default" disabled="disabled" name="username" id="username"><br>
            <label for="password">Password</label><br>
            <input class="ui-wizard-content ui-helper-reset ui-state-default" disabled="disabled" name="password" id="password" type="password"><br>
            <label for="retypePassword">Retype password</label><br>
            <input class="ui-wizard-content ui-helper-reset ui-state-default" disabled="disabled" name="retypePassword" id="retypePassword" type="password"><br>
          </span>
          </div>
          <div id="demoNavigation">                             
            <input class="ui-wizard-content ui-helper-reset ui-state-default ui-formwizard-button ui-state-disabled" disabled="disabled" id="back" value="Back" type="reset">
            <input class="ui-wizard-content ui-helper-reset ui-state-default ui-formwizard-button ui-state-active" id="next" value="Next" type="submit">
          </div>
        </form>

