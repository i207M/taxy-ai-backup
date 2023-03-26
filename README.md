<img src="src/assets/img/icon-128.png" width="64"/>

# Web Agent Browser Extension

Web Agent is a Chrome Extension that sends a simplified version of the browser's DOM to GPT-4 along with a user-specified task to accomplish, then utilizes Chrome's built-in extension APIs to interact with the browser to perform the actions GPT-4 suggests.

## Table of Contents

- [Web Agent Browser Extension](#web-agent-browser-extension)
  - [Table of Contents](#table-of-contents)
  - [How it Works](#how-it-works)
  - [Simple Demos](#simple-demos)
    - [Searching for and playing the movie Oblivion in Netflix](#searching-for-and-playing-the-movie-oblivion-in-netflix)
    - [Creating a calendar event](#creating-a-calendar-event)
    - [Writing an essay in the OpenAI playground](#writing-an-essay-in-the-openai-playground)
    - [Add your own!](#add-your-own)
  - [Installing and Running](#installing-and-running)
    - [Installing the extension](#installing-the-extension)
    - [Running in your browser](#running-in-your-browser)
  - [Tech Stack](#tech-stack)
  - [Resources](#resources)

## How it Works

1. Web Agent runs a content script on the browser page to pull the entire DOM. It simplifies it to only include interactive or contextual elements, like text. It assigns an id to each interactive element.
2. Web Agent sends the simplified DOM, along with the user's instructions, to the selected LLM (currently limited to GPT-3.5 and GPT-4). Web Agent informs the LLM of two potential actions to interact with the webpage:
   1. `click(id)` - click on the interactive element associated with that id
   2. `setValue(id, text)` - focus on a text input, clear its existing text, and type the provided text into that input
3. If possible, Web Agent parses the LLM's response for an action to complete, and then takes that action using the [chrome.debugger API](https://developer.chrome.com/docs/extensions/reference/debugger/). This could fail to happen for three reasons:
   1. The LLM completed the provided task. Before each action, the LLM will evaluate if the user-specified task is complete, and shut itself down if so.
   2. The user stopped the task's execution. The user can stop the LLM's execution at any time, without waiting for it to be completed.
   3. There was an error. Web Agent's safety-first policy causes it to automatically stop execution in the event of an unexpected error.
4. Once the action is complete and assuming that the task is not completed, Web Agent will cycle back to step 1 and parse the updated DOM. Web Agent can currently complete a maximum of 50 actions for a task, though in practice most tasks require fewer than 10 actions.

## Simple Demos

### Searching for and playing the movie Oblivion in Netflix

![Oblivion](https://user-images.githubusercontent.com/41524992/227739533-9c8711b0-ed21-4b28-9099-823a0d2a4db2.gif)

### Creating a calendar event

![Calendar](https://user-images.githubusercontent.com/41524992/227739258-e4721e54-1f97-46e2-a50e-36580ef26bc7.gif)

### Writing an essay in the OpenAI playground

![playground](https://user-images.githubusercontent.com/41524992/227739246-53cb4587-6fa1-491f-80fa-f05ec0b13967.gif)

### Add your own!

If you have an interesting demo you'd like to share, submit a PR to add your own!

## Installing and Running

Currently this extension is only available through the GitHub repo. We plan to release on the Chrome Web Store after fixing some known bugs and adding features to increase its appeal to a non-technical audience. To build and install the extension locally on your machine, follow the instructions below.

### Installing the extension

1. Check if your [Node.js](https://nodejs.org/) version is >= **16**.
2. Clone this repository.
3. Run `yarn` to install the dependencies.
4. Run `yarn start` to build the package
5. Load your extension on Chrome by doing the following:
   1. Navigate to `chrome://extensions/`
   2. Toggle `Developer mode`
   3. Click on `Load unpacked extension`
   4. Select the `build` folder that `yarn start` generated.

### Running in your browser

1. Once installed, the browser plugin will be available in two forms:
   1. As a Popup. Activate by pressing `cmd+shift+y` on mac or `ctrl+shift+y` on windows/linux, or by clicking the extension logo in your browser.
   2. As a devtools panel Activate by first opening the browser's developer tools, then navigating to the `Web Agent` panel.
2. The next thing you need to do is create or access an existing [OpenAI API Key](https://platform.openai.com/account/api-keys) and paste it in the provided prompt. This key will be stored securely in your browser, not uploaded to any external server.
3. Finally, navigate to a webpage you want Web Agent to act upon (for instance the [OpenAI playground](https://platform.openai.com/playground)) and start experimenting!

## Tech Stack

Technology currently used by this extension:

- [Chrome Extension Manifest V3](https://developer.chrome.com/docs/extensions/mv3/intro/mv3-overview/)
- [React 17](https://reactjs.org)
- [Webpack 5](https://webpack.js.org/)
- [Webpack Dev Server 4](https://webpack.js.org/configuration/dev-server/)
- [React Hot Loader](https://github.com/gaearon/react-hot-loader)
- [eslint-config-react-app](https://www.npmjs.com/package/eslint-config-react-app)
- [Prettier](https://prettier.io/)
- [TypeScript](https://www.typescriptlang.org/)

## Resources

- [Getting Started with Chrome Extensions](https://developer.chrome.com/extensions/getstarted)