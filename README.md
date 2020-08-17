# react-hwe-editor

> React based implementation of HELM Web Editor. 

# HELM 
HELM (hierarchical editing language for macromolecules) is both a notation, and a set of tools and applications that implement the notation. It allows (among other things) the compact representation of complex biomolecules, includes the ability to use non-natural monomers, enables you to create conjugates of different types of polymers or small molecules and describe ambiguity. 

Our [wiki](https://pistoiaalliance.atlassian.net/wiki/spaces/PUB/pages/8716303/HELM+Resources) is the best place to start if you have a general interest in HELM. You can get easy access to compiled versions of the tools, links to the documentation including slide sets, videos and information about the community and project. 

Developers will find all the open source code under this PistoiaHELM organisation. 

We have an active group which meets regularly and works on extending the tools and supporting HELM. We love it when new people want to get involved, so please do get in touch. 

Email us at info@openHELM.org  

# HELM WebEditor 

The HELM Web Editor react (HWE) is a react based implementation of HELM. The webeditor depends on a set of services which have been split into modules for easier maintenance. The diagram below shows the relationship between the components. 

![](https://github.com/ClairePA/pistoiahelm.github.com/blob/master/images/ArchitectureOverview.png?raw=true)

## Install

```bash
npm install --save styled-components
npm install --save reactjs-popup
npm install --save @pistoiahelm/react-hwe-editor --registry=https://npm.pkg.github.com
```

## Link to NPM Package
> [react-hwe-editor](https://github.com/PistoiaHELM/react-hwe-editor/packages/317570) 

# useHWE #

The useHWE react hook has several parameters to note (examples in the **Usage** section below): 

 *   initHelm: input helm notation to be rendered/analyzed by HWE
 *   customConfig: custom configuration settings for HWE  

The following is a link to a much more in-depth description of the inputs and outputs for the useHWE hook:
> [Guide to useHWE hook](useHWE.md)

If you want to use the default urls for monomer dbs, include a proxy to http://webeditor.openhelm.org in your own project's package.json: 

```js
"proxy": "http://webeditor.openhelm.org"
```

## Usage

### Example 1: Basic Editor App
```jsx
/**
 * Basic example react App using editor react component
 */
import React from 'react';
import { useHWE } from 'react-hwe-editor';

const App = () => {
    const { Editor, editorProps } = useHWE('helm');

    return(
        <div className='App'>  
            <h1>Welcome to my HELM Web Editor App!</h1>
            <Editor {...editorProps}/>
        </div>
    );
}

export default App;
``` 

### Example 2: Basic Viewer App
```jsx
/**
 * Basic example react App using viewer react component
 */
import React from 'react';
import { useHWE } from 'react-hwe-editor';

const App = () => {
    const { Viewer, viewerProps } = useHWE('helm');

    return(
        <div className='App'>  
            <h1>Welcome to my HELM Web Editor Viewer App!</h1>
            <Viewer {...viewerProps}/>
        </div>
    );
}

export default App;
``` 

### Example 3: Custom Data Handling
```jsx
/**
 * More complex usage case handling viewer data
 */
import React, { useState } from 'react';
import { useHWE } from 'react-hwe-editor';

const App = () => {
    const { Viewer, viewerProps } = useHWE('helm');
    const [myMW, setMyMW] = useState('');

    // details about all viewerProps 
    viewerProps.displayMolecularProperties = false;
    viewerProps.viewerCallback = (data) => {
        let molecularProps = JSON.parse(data.molecularProps);
        setMyMW(molecularProps.mw);
    }

    return(
        <div className='App'>  
            <h1>Welcome to my HELM Web Editor Viewer App!</h1>
            <h4>Current Molecular Weight: {myMW} </h4>
            <Viewer {...viewerProps}/>
        </div>
    );
}
``` 

### Example 4: Multiple Viewers
```jsx
/**
 * More complex usage case handling multiple viewers and callbacks
 */
import React, {useState} from 'react';
import { useHWE } from 'react-hwe-editor';

const App = () => {
    const { Viewer, viewerProps } = useHWE('helm');

    const [myHELM, setMyHELM] = useState();
    const [myHELM2, setMyHELM2] = useState();
    const [myMW, setMyMW] = useState();

    const myCallback = (data)=> {
        setMyHELM(data.helm)
    }

    const myCallback2 = (data)=> {
        setMyHELM2(data.helm)
    }

    const myCallback3 = (data) => {
        const molecularProps = JSON.parse(data.molecularProps);
        setMyMW(molecularProps.mw)
    }

    viewerProps.viewerCallback = myCallback;
    const viewerProps2 = {...viewerProps, ...{viewerCallback: myCallback2, initHELM: 'helmhelm'}}
    const viewerProps3 = {...viewerProps, ...{viewerCallback: myCallback3, initHELM: 'asdf'}}

    return(
        <div className='App'>  
            <h1>Welcome to my HELM Web Editor Viewer App!</h1>
            <h4>Current HELM for viewer 1: {myHELM} </h4>
            <h4>Current HELM for viewer 2: {myHELM2} </h4>
            <h4>Current molecular weight for viewer 3: {myMW} </h4>
            <Viewer {...viewerProps} />
            <Viewer {...viewerProps2} />
            <Viewer {...viewerProps3} />
        </div>
    );
}
export default App;
``` 

Reference the guide to the useHWE hook to see more examples and an in-depth description of the inputs and outputs for the useHWE hook:
> [Guide to useHWE hook](useHWE.md)

[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## License

MIT © [Visininjr](https://github.com/Visininjr)