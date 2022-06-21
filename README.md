# IntegralUI Web - Toaster, v22.2

IntegralUI Web - Toaster is a native Web Component that allows you to display notification messages (Toasts) with different alert levels. 

![IntegralUI Web - Toaster, 22.2 - a native Web Component for JavaScript, Angular, React and Vue. Allows you to display notification messages (Toasts) with different alert levels.](https://www.lidorsystems.com/products/web/studio/images/integralui-web-toaster.png)

<b>Note</b> This component is part of [IntegralUI Web](https://github.com/lidorsystems/integralui-web.git) library.

Here is a brief overview of what is included:


## Components

[Toaster](https://www.lidorsystems.com/products/web/studio/samples/#/toaster) - Allows you to display notification messages (Toasts) with different alert levels</li>


## Services

<b>Common</b> - Includes a set of common functions usable in most applications


## Dependencies

IntegralUI Web is built on top of [LitElement](https://github.com/Polymer/lit-element). All necessary files from that library are already included in the /external subfolder of this repository.


## DEMO

[Online QuickStart App](https://www.lidorsystems.com/products/web/studio/samples/) - An online demo of Toaster component is included


## Installation

Install the repository by running

```bash
npm install https://github.com/lidorsystems/integralui-web-toaster.git
```

or directly from NPM

```bash
npm i integralui-web-toaster
```


## How to Use

<b>Note</b> A detailed information is available here: [How to Use IntegralUI Web Components](https://www.lidorsystems.com/help/integralui/web-components/introduction/installation/). Explains how to setup and use components for each framework: Angular, React or Vanilla JavaScript.

In general, you need to open your application and add a reference to a component you want to use. For example, if you are using the IntegralUI Toaster component:</p>

### Angular

```bash
import 'integralui-web-toaster/components/integralui.toaster.js';
import { IntegralUITheme, IntegralUIToastAlignment, IntegralUIToastType } from 'integralui-web-toaster/components/integralui.enums.js';

@Component({
    selector: '',
    templateUrl: './toaster-overview.html',
    styleUrls: ['./toaster-overview.css']
})
export class ToasterOverviewSample {
    @ViewChild('toaster', { static: false }) toaster: ElementRef;

    public currentResourcePath: string = 'app/integralui/icons';
    public currentTheme: IntegralUITheme = IntegralUITheme.Office;
    public currentToastAlignment: any = null;
    public currentToastType: any = null;
    public toastDuration: number = 5000;
    public toastMessage: string = 'Sample message';
    public toastTitle: string = 'Info';
    public typeOptions: Array<any> = [
        { text: 'Error' },
        { text: 'Info' },
        { text: 'Success' },
        { text: 'Warning' }
    ];

    // . . .

    getToastAlignment(){
        return this.currentToastAlignment ? this.currentToastAlignment.text : IntegralUIToastAlignment.Info;
    }
   
    onShowClicked(){
        this.toaster.nativeElement.show({ title: this.toastTitle, text: this.toastMessage, type: this.currentToastType ? this.currentToastType.text : IntegralUIToastType.Info });
    }

```

Then, place the component in HTML using its tag. Here is an example:


```bash
<iui-toaster #toaster
    [alignment]="getToastAlignment()"
    [allowAnimation]="true"
    [contentTemplate]="currentContentTemplate"
    [duration]="toastDuration"
    [parentId]="'toaster-overview'"
    [theme]="currentTheme"
></iui-toaster>
```

To add custom HTML elements, you can create a template using LitElement html:

```bash
import { html } from 'integralui-web-toaster/external/lit-element.js';
import { styleMap } from 'integralui-web-toaster/external/style-map.js';

currentContentTemplate = (toast: any) => {
    return html`<div>
            <div style=${styleMap({ fontWeight: 'bold', marginBottom: '10px', position: 'relative' })}>
                <span>${this.toastTitle}</span>
                <span style=${styleMap({ position: 'absolute', fontSize: '1.5rem', right: 0, top: '-4px' })} @click="${() => this.hideToast(toast)}">&times;</span>
            </div>
            <hr style=${styleMap({ background: '#d9d9d9', border: 0, height: '1px' })} />
            <span>${toast.text}</span>
        </div>`;
}

hideToast(toast: any){
    this.toaster.nativeElement.hide(toast);
}
```

Depending on current version of TypeScript, you may need to add some settings in tsconfig.json, under "angularCompilerOptions":

```bash
"angularCompilerOptions": {

    . . .

    "suppressImplicitAnyIndexErrors": true,     // solves implicit any values
    "noImplicitAny": false,             // solves angular could not find a declaration file for module implicitly has an 'any' type
    "strictNullChecks": false           // solves type null is not assignable to type

}
```


### React

Currently [ReactJS doesn't have full support for Web Components](https://custom-elements-everywhere.com/#react). Mainly because of the way data is passed to the component via attributes and their own synthetic event system. For this reason, you can use available wrappers located under /wrappers directory, which are ReactJS components that provide all public API from an IntegralUI component.</p>

```bash
import IntegralUIToasterComponent from 'integralui-web-toaster/wrappers/react.integralui.toaster.js';
import { IntegralUITheme, IntegralUIToastAlignment, IntegralUIToastType } from 'integralui-web-toaster/components/integralui.enums.js';

class ToasterOverview extends Component {

    //
    // Initialization / Destruction --------------------------------------------------------------
    //

    constructor(props){
        super(props);

        this.state = {
            alignmentOptions: [
                { text: 'BottomCenter' },
                { text: 'BottomFull' },
                { text: 'BottomLeft' },
                { text: 'BottomRight' },
                { text: 'TopCenter' },
                { text: 'TopFull' },
                { text: 'TopLeft' },
                { text: 'TopRight' }
            ],
            isAnimationAllowed: true,
            isTemplateActive: false,
            labelContentSize: { width: 300 },
            currentResourcePath: 'integralui/icons',
            currentTheme: IntegralUITheme.Office,
            toastDuration: 5000,
            toastMessage: 'Sample message',
            toastTitle: 'Info',
            typeOptions: [
                { text: 'Error' },
                { text: 'Info' },
                { text: 'Success' },
                { text: 'Warning' }
            ]
        }

        this.state.currentToastAlignment = this.state.alignmentOptions[7];
        this.state.currentToastType = this.state.typeOptions[1];
        
        this.toasterRef = React.createRef();
    }

    // . . .

    onShowClicked(){
        this.toasterRef.current.show({ title: this.state.toastTitle, text: this.state.toastMessage, type: this.state.currentToastType ? this.state.currentToastType.text : IntegralUIToastType.Info });
    }
```

Then, place the component in HTML using its tag. Here is an example:

```bash
render() {
    return (
        <div id="toaster-overview" style={{ overflow: 'hidden', position: 'relative' }}>
            <IntegralUIToasterComponent ref={this.toasterRef}
                alignment={this.state.currentToastAlignment ? this.state.currentToastAlignment.text : IntegralUIToastAlignment.TopRight }
                allowAnimation={this.state.isAnimationAllowed}
                contentTemplate={this.currentContentTemplate}
                duration={this.state.toastDuration}
                parentId={'toaster-overview'}
                theme={this.state.currentTheme}
            ></IntegralUIToasterComponent>
        </div>
    );
}
```

To add custom HTML elements, you can create a template using LitElement html:

```bash
import { html } from 'integralui-web-toaster/external/lit-element.js';
import { styleMap } from 'integralui-web-toaster/external/style-map.js';

currentContentTemplate = (toast) => {
    return html`<div>
            <div style=${styleMap({ fontWeight: 'bold', marginBottom: '10px', position: 'relative' })}>
                <span>${this.state.toastTitle}</span>
                <span style=${styleMap({ position: 'absolute', fontSize: '1.5rem', right: 0, top: '-4px' })} @click="${() => this.hideToast(toast)}">&times;</span>
            </div>
            <hr style=${styleMap({ background: '#d9d9d9', border: 0, height: '1px' })} />
            <span>${toast.text}</span>
        </div>`;
}


hideToast(toast){
    this.toasterRef.current.hide(toast);
}
```


### Vanilla JavaScript

```bash
<script type="module" src="integralui-web-toaster/components/integralui.toaster.js"></script>
```

Then, place the component in HTML using its tag. Here is an example:

```bash
<div class="toaster-block">
    <iui-toaster allow-animation="true" theme="Office"></iui-toaster>
</div>
```

To add custom HTML elements, you can create a template using LitElement html:

```bash
import { html } from 'integralui-web-toaster/external/lit-element.js';
import { styleMap } from 'integralui-web-toaster/external/style-map.js';

// Get a reference to the Toaster component
const toaster = document.querySelector('iui-toaster');

toaster.contentTemplate = function(toast){
    return html`<div>
            <div style=${styleMap({ fontWeight: 'bold', marginBottom: '10px', position: 'relative' })}>
                <span>${toast.title}</span>
                <span style=${styleMap({ position: 'absolute', fontSize: '1.5rem', right: 0, top: '-4px' })} @click="${(e) => hideToast(toast)}">&times;</span>
            </div>
            <hr style=${styleMap({ background: '#dedede', border: 0, height: '1px' })} />
            <span>${toast.text}</span>
        </div>`;
}

let hideToast = function(toast){
    toaster.hide(toast);
}
```

## QuickStart App

There is a demo application with source code that contains samples for each component included in the IntegralUI Web library. It can help you to get started quickly with learning about the components and write tests immediatelly. 

From [IntegralUI Web - QuickStart](https://github.com/lidorsystems/integralui-web-quickstart) you can download a demo app for Angular, AngularJS, React and Vanilla JavaScript. A detailed information about each of these quick-start demos is available in ReadMe file, located in the root folder of the demo app.


## License Information

You are FREE to use this product to develop Internet and Intranet web sites, web applications and other products, with no-charge.

This project has been released under the IntegralUI Web Lite License, and may not be used except in compliance with the License.
A copy of the License should have been installed in the product's root installation directory or it can be found here: [License Agreement](https://www.lidorsystems.com/products/web/lite/integralui-web-lite-license-agreement.pdf).

This SOFTWARE is provided "AS IS", WITHOUT WARRANTY OF ANY KIND, either express or implied. See the License for the specific language governing rights and limitations under the License.