Having been working on email for the past 3-4 years I have regrettably not been keeping up with my frontend development stacks. Things have changed dramatically since 2014, particularly the traction that React & React-native have gotten at startups these days. Nearly every job description that I see mentions React.

Teaching an old dog new tricks

First things first, I had to figure out how to get any of this to install.

I need multiple versions of `node.js` on my computer so I installed `nvm`. I had a hell of a time getting even the "Getting Started" steps for React-native to run properly. My version of Node was too old, then my version of NPM was too new. Versioning is a big roadblock on the process and they don't explain it much in the docs. 

As of this writing 4/2018, I have arrived at this environment. I'm sure it changes frequently.
* React-native 0.52 `npm install react-native`
* React 16.2 (came as dependency for react-native)
* Node 8.1.0 `nvm install 8.1.0`
* NPM 4.6.1 (need to look up how I got this installed. For node 8.1.0 I had to downgrade npm for compatibility)
* Yarn 1.5.1 (maybe I do not need npm if I have yarn) `brew install yarn --without-node`

Adding other useful tools:
* Atom https://atom.io/
* Nuclide for Atom https://nuclide.io/
