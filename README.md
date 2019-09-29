# React Native Router (v4.x) [![Backers on Open Collective](https://opencollective.com/react-native-router-flux/backers/badge.svg)](#backers) [![Sponsors on Open Collective](https://opencollective.com/react-native-router-flux/sponsors/badge.svg)](#sponsors) [![Join the chat at https://gitter.im/aksonov/react-native-router-flux](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/aksonov/react-native-router-flux?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Codacy Badge](https://api.codacy.com/project/badge/grade/c6d869e2367a4fb491efc9de228c5ac6)](https://www.codacy.com/app/aksonov-github/react-native-router-flux) [![npm version](https://badge.fury.io/js/react-native-router-flux.svg)](http://badge.fury.io/js/react-native-router-flux) [![CircleCI](https://circleci.com/gh/aksonov/react-native-router-flux.svg?style=svg)](https://circleci.com/gh/aksonov/react-native-router-flux)

[Follow author @PAksonov](https://twitter.com/PAksonov)

`react-native-router-flux` is a different API over `react-navigation`. It helps users to define all the routes in one central place and navigate and communicate between different screens in an easy way. But it also means that `react-native-router-flux` inherits all [limitations](https://reactnavigation.org/docs/en/limitations.html) and changes from updated versions.

### IMPORTANT NOTES
#### v4.1.0-beta.x is based on [React Navigation v3.x](https://reactnavigation.org/)
#### v4 is based on [React Navigation v2.x]. See [this branch](https://github.com/aksonov/react-native-router-flux/tree/v3) and [docs](https://github.com/aksonov/react-native-router-flux/blob/master/README3.md) for v3 based on deprecated React Native Experimental Navigation API. It is not supported and may not work with latest React Native versions.

#### v4.0.0-beta.x is based on [React Navigation v1.5.x](https://reactnavigation.org/). See [this branch](https://github.com/aksonov/react-native-router-flux/tree/v4.0.0-beta) for this version. It is also not supported and may not work with the latest React Native versions.

___

* [Examples](#try-the-example-apps)
* [Motivation](https://gist.github.com/aksonov/e2d7454421e44b1c4c72214d14053410)
* [v4 Features](#v4-features)
* [API](/docs/API.md)
* [Migrating from v3](/docs/MIGRATION.md)
* [Sponsors/Backers/Contributors](#contributors)


## Usage

Define all your routes in one React component...

```js
const App = () => (
  <Router>
    <Stack key="root">
      <Scene key="login" component={Login} title="Login"/>
      <Scene key="register" component={Register} title="Register"/>
      <Scene key="home" component={Home}/>
    </Stack>
  </Router>
);
```

...and navigate from one scene to another scene with a simple and powerful API.

```js
// Login.js

// navigate to 'home' as defined in your top-level router
Actions.home(PARAMS)

// go back (i.e. pop the current screen off the nav stack)
Actions.pop()

// refresh the current Scene with the specified props
Actions.refresh({param1: 'hello', param2: 'world'})
```

## API

For a full listing of the API, [view the API docs](https://github.com/aksonov/react-native-router-flux/blob/master/docs/API.md).

## Try the [example apps](https://github.com/aksonov/react-native-router-flux/tree/master/examples)

![rnrf](https://user-images.githubusercontent.com/3681859/27937441-ef61d932-626b-11e7-885f-1db7dc74b32e.gif)

```bash
# Get the code
git clone https://github.com/aksonov/react-native-router-flux.git
cd react-native-router-flux/examples/[expo|react-native|redux]

# Installing dependencies
yarn

# Run it
yarn start
```

## v4 Features
* Based on latest [React Navigation](https://reactnavigation.org) API
* Separate navigation logic from presentation. You may now change navigation state directly from your business logic code - stores/reducers/etc. navigationStore
* Built-in state machine (v3 `Switch` replacement)
  * Each `Scene` with `component` defined can have `onEnter`/`onExit`/`on` handlers.
  * `onEnter`/`on` handler can be async.
  * For 'truthy' return of `onEnter`/`on`, `success` handler (if defined) will be executed
    * if `success` is a string then router will navigate to the `Scene` with that key
  * in case of handler's failure, `failure` prop (if defined) will be run.
  * Combining `onEnter`, `onExit`, `success`, and `failure` makes patterns like authentication, data validation, and conditional transitions simple and intuitive.
* [MobX](https://mobx.js.org/)-friendly: all scenes are wrapped with `observer`. You may subscribe to `navigationStore` (`Actions` in v3) and observe current navigation state. Not applicable to Redux.
* Flexible Nav bar customization, currently not allowed by React Navigation:
https://github.com/react-community/react-navigation/issues/779
* Drawer support (provided by React Navigation)
* Inheritance of scene attributes allow you to avoid any code/attribute duplications. Adding `rightTitle` to a scene will apply to all child scenes simultaneously. See example app.
* Access to your app navigations state as simple as `Actions.state`.
* Use `Actions.currentScene` to get name of current scene.

### Helpful tips if you run into some gotchas

## Using Static on Methods with HOCs
* This is just a helpful tip for anyone who use the onExit/onEnter methods as a static method in their Component Class. Please refer to this link https://reactjs.org/docs/higher-order-components.html.

* If your Scene Components are Wrapped in Custom HOCs/ Decorators, then the static onExit/onEnter methods will not work as your Custom HOCs will not copy the static methods over to your Enhanced Component.Use the npm package called hoist-non-react-statics to copy your Component level static methods over to your Enhanced Component.

## Implement onBack from your Scene after declaring it

* If you have a Scene where in you want to make some changes to your Component State when Back button is pressed, then doing this 
```js
<Scene key={...} component={...} onBack={()=>{/*code*/}}/>
``` 
will not help.

```js
<Scene key={...} component={...} onBack={()=>{/*code*/}} back={true}/> 
```
Make sure back = true is passed to your scene, now in your Component's lifecycle add this

```js
componentDidMount(){
    InteractionManager.runAfterInteractions(()=> {
        Actions.refresh({onBack:()=>this.changeSomethingInYourComponent()})
    })
}
```


## Contributors

This project exists thanks to all the people who contribute. [[Contribute]](CONTRIBUTING.md).
<a href="https://github.com/aksonov/react-native-router-flux/graphs/contributors"><img src="https://opencollective.com/react-native-router-flux/contributors.svg?width=890" /></a>


## Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/react-native-router-flux#backer)]

<a href="https://opencollective.com/react-native-router-flux#backers" target="_blank"><img src="https://opencollective.com/react-native-router-flux/backers.svg?width=890"></a>


## Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/react-native-router-flux#sponsor)]

<a href="https://opencollective.com/react-native-router-flux/sponsor/0/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/1/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/2/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/3/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/4/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/5/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/6/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/7/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/8/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/react-native-router-flux/sponsor/9/website" target="_blank"><img src="https://opencollective.com/react-native-router-flux/sponsor/9/avatar.svg"></a>



Apache License
                           Version 2.0, January 2004
                        https://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

   1. Definitions.

      "License" shall mean the terms and conditions for use, reproduction,
      and distribution as defined by Sections 1 through 9 of this document.

      "Licensor" shall mean the copyright owner or entity authorized by
      the copyright owner that is granting the License.

      "Legal Entity" shall mean the union of the acting entity and all
      other entities that control, are controlled by, or are under common
      control with that entity. For the purposes of this definition,
      "control" means (i) the power, direct or indirect, to cause the
      direction or management of such entity, whether by contract or
      otherwise, or (ii) ownership of fifty percent (50%) or more of the
      outstanding shares, or (iii) beneficial ownership of such entity.

      "You" (or "Your") shall mean an individual or Legal Entity
      exercising permissions granted by this License.

      "Source" form shall mean the preferred form for making modifications,
      including but not limited to software source code, documentation
      source, and configuration files.

      "Object" form shall mean any form resulting from mechanical
      transformation or translation of a Source form, including but
      not limited to compiled object code, generated documentation,
      and conversions to other media types.

      "Work" shall mean the work of authorship, whether in Source or
      Object form, made available under the License, as indicated by a
      copyright notice that is included in or attached to the work
      (an example is provided in the Appendix below).

      "Derivative Works" shall mean any work, whether in Source or Object
      form, that is based on (or derived from) the Work and for which the
      editorial revisions, annotations, elaborations, or other modifications
      represent, as a whole, an original work of authorship. For the purposes
      of this License, Derivative Works shall not include works that remain
      separable from, or merely link (or bind by name) to the interfaces of,
      the Work and Derivative Works thereof.

      "Contribution" shall mean any work of authorship, including
      the original version of the Work and any modifications or additions
      to that Work or Derivative Works thereof, that is intentionally
      submitted to Licensor for inclusion in the Work by the copyright owner
      or by an individual or Legal Entity authorized to submit on behalf of
      the copyright owner. For the purposes of this definition, "submitted"
      means any form of electronic, verbal, or written communication sent
      to the Licensor or its representatives, including but not limited to
      communication on electronic mailing lists, source code control systems,
      and issue tracking systems that are managed by, or on behalf of, the
      Licensor for the purpose of discussing and improving the Work, but
      excluding communication that is conspicuously marked or otherwise
      designated in writing by the copyright owner as "Not a Contribution."

      "Contributor" shall mean Licensor and any individual or Legal Entity
      on behalf of whom a Contribution has been received by Licensor and
      subsequently incorporated within the Work.

   2. Grant of Copyright License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      copyright license to reproduce, prepare Derivative Works of,
      publicly display, publicly perform, sublicense, and distribute the
      Work and such Derivative Works in Source or Object form.

   3. Grant of Patent License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      (except as stated in this section) patent license to make, have made,
      use, offer to sell, sell, import, and otherwise transfer the Work,
      where such license applies only to those patent claims licensable
      by such Contributor that are necessarily infringed by their
      Contribution(s) alone or by combination of their Contribution(s)
      with the Work to which such Contribution(s) was submitted. If You
      institute patent litigation against any entity (including a
      cross-claim or counterclaim in a lawsuit) alleging that the Work
      or a Contribution incorporated within the Work constitutes direct
      or contributory patent infringement, then any patent licenses
      granted to You under this License for that Work shall terminate
      as of the date such litigation is filed.

   4. Redistribution. You may reproduce and distribute copies of the
      Work or Derivative Works thereof in any medium, with or without
      modifications, and in Source or Object form, provided that You
      meet the following conditions:

      (a) You must give any other recipients of the Work or
          Derivative Works a copy of this License; and

      (b) You must cause any modified files to carry prominent notices
          stating that You changed the files; and

      (c) You must retain, in the Source form of any Derivative Works
          that You distribute, all copyright, patent, trademark, and
          attribution notices from the Source form of the Work,
          excluding those notices that do not pertain to any part of
          the Derivative Works; and

      (d) If the Work includes a "NOTICE" text file as part of its
          distribution, then any Derivative Works that You distribute must
          include a readable copy of the attribution notices contained
          within such NOTICE file, excluding those notices that do not
          pertain to any part of the Derivative Works, in at least one
          of the following places: within a NOTICE text file distributed
          as part of the Derivative Works; within the Source form or
          documentation, if provided along with the Derivative Works; or,
          within a display generated by the Derivative Works, if and
          wherever such third-party notices normally appear. The contents
          of the NOTICE file are for informational purposes only and
          do not modify the License. You may add Your own attribution
          notices within Derivative Works that You distribute, alongside
          or as an addendum to the NOTICE text from the Work, provided
          that such additional attribution notices cannot be construed
          as modifying the License.

      You may add Your own copyright statement to Your modifications and
      may provide additional or different license terms and conditions
      for use, reproduction, or distribution of Your modifications, or
      for any such Derivative Works as a whole, provided Your use,
      reproduction, and distribution of the Work otherwise complies with
      the conditions stated in this License.

   5. Submission of Contributions. Unless You explicitly state otherwise,
      any Contribution intentionally submitted for inclusion in the Work
      by You to the Licensor shall be under the terms and conditions of
      this License, without any additional terms or conditions.
      Notwithstanding the above, nothing herein shall supersede or modify
      the terms of any separate license agreement you may have executed
      with Licensor regarding such Contributions.

   6. Trademarks. This License does not grant permission to use the trade
      names, trademarks, service marks, or product names of the Licensor,
      except as required for reasonable and customary use in describing the
      origin of the Work and reproducing the content of the NOTICE file.

   7. Disclaimer of Warranty. Unless required by applicable law or
      agreed to in writing, Licensor provides the Work (and each
      Contributor provides its Contributions) on an "AS IS" BASIS,
      WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
      implied, including, without limitation, any warranties or conditions
      of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
      PARTICULAR PURPOSE. You are solely responsible for determining the
      appropriateness of using or redistributing the Work and assume any
      risks associated with Your exercise of permissions under this License.

   8. Limitation of Liability. In no event and under no legal theory,
      whether in tort (including negligence), contract, or otherwise,
      unless required by applicable law (such as deliberate and grossly
      negligent acts) or agreed to in writing, shall any Contributor be
      liable to You for damages, including any direct, indirect, special,
      incidental, or consequential damages of any character arising as a
      result of this License or out of the use or inability to use the
      Work (including but not limited to damages for loss of goodwill,
      work stoppage, computer failure or malfunction, or any and all
      other commercial damages or losses), even if such Contributor
      has been advised of the possibility of such damages.

   9. Accepting Warranty or Additional Liability. While redistributing
      the Work or Derivative Works thereof, You may choose to offer,
      and charge a fee for, acceptance of support, warranty, indemnity,
      or other liability obligations and/or rights consistent with this
      License. However, in accepting such obligations, You may act only
      on Your own behalf and on Your sole responsibility, not on behalf
      of any other Contributor, and only if You agree to indemnify,
      defend, and hold each Contributor harmless for any liability
      incurred by, or claims asserted against, such Contributor by reason
      of your accepting any such warranty or additional liability.

   END OF TERMS AND CONDITIONS

   APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "[]"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

   Copyright [2019] [Rolando Gopez Lacuata]

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       https://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
