# Simplifying import Statements With Module Path Aliases

I was added to a team project which was built with react a few months ago. I found a little awkward thing in the project files. Almost all the files had to import assets like css, images, logos, svg, icons, fonts etc - which is normal - but the awkward thing is the repetitive way in which files are being imported. Below is an excerpt from a file in the project. 

NB : Notice the `from "../../components", "../../assets/icons", "../../assets/images"` etc

```
import Grid from '../../components/Grid';
import MainWrapper from './components/MainWrapper';
import DashboardMockup from '../../assets/images/mockups/dashboard_mockup.png';
import { ReactComponent as RightArrow } from '../../assets/icons/RightArrow.svg';
import { ReactComponent as Cards } from '../../assets/icons/cards.svg';
import { ReactComponent as MoneyChange } from '../../assets/icons/money-change.svg';
import { ReactComponent as Arrow } from '../../assets/icons/Arrow.svg';
import { GreenBold, H1, P } from '../../components/Texts'
import waitlist_css from '../../assets/css/modules/waitlist.module.css'
import { margin } from '../../assets/styles';
import FloatingStar from './components/FloatingStar';
import { error, showToast, success } from '../../utils/functions';
import isEmail from 'validator/lib/isEmail';
import FooterLinks from './components/FooterLinks';
import styled from 'styled-components'
import { PrimaryButton } from '../../components/Buttons';
import axios from 'axios'

```

You might be wondering, why do you have to repeatedly type out the same boring codes so many times? Well, you should. But the goodnews is, you don't have to go through this, because Path Aliases has got your back.

<img src="https://raw.githubusercontent.com/b4b4r07/screenshots/master/gotcha/logo.png" alt="gotcha"></a> 
 
## Module Path Aliases In Next.js

This article shows how to get past this repetitive, time wasting and ridiculously boring act for projects using nextjs with either javascript or typescript. To learn how to do this in other projects, you can learn how to do this in pure react projects [here](https://github.com/borguuh/blogs/blob/main/article-1/article-1-solution.md), or for a typical typescript project [here](https://github.com/borguuh/blogs/blob/main/article-1/article-1-solution.md) and for any nodejs project [here](https://github.com/borguuh/blogs/blob/main/article-1/article-1-solution.md).

Next.js supports module path aliasing out of the box without much configurations. Depending on whether your project is based on typescript or javascript. You just have to create a jsconfig.json or tsconfig.json file in the root folder if you don't have it already. Then add a few lines of code specifying paths/folders to assets in your project.

## jsconfig.json OR tsconfig.json

These are files that hold the configuration settings for the js/ts files in a project structure. They basicaly dictate or override how javascript/typescript codes should be treated for the project in question. They are automatically recognized as the configuration files once found in the project root folder. 

So, rather than to spend the bulk of time trying analyze where to find each file or module to import them manually by going two or more level up or down so as to get the specific file or function, these config files do the heavy lifting for you. If this is not done, things will surely get messier and very boringly tiring as a codebase grows.

## Creating module aliases with jsconfig.json / tsconfig.json

There are three config properties to be aware of to achieve this. These are the `compilerOptions`, `baseUrl` and the `paths` properties.

It is as simple as creating a json object in it with `compilerOptions` key. Then specifying the project base url with the "baseUrl" option, then the various paths to each file you want to reference with the `paths` option while using the reference key value pair, an example is as shown below. 

```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/components/*": ["components/*"],
      "@/styles/*": ["styles/*"],
      "@/config/*": ["config/*"],
      "@/context/*": ["context/*"],
      "@/helpers/*": ["helpers/*"]
    }
  }
}

```

In the example above, there five (5) folders named components, context, config, styles and helpers. importing from the "@/components" will automatically go to the `/components` folder wherever you are in the file tree.

## Calling reference paths by their alias

To import any file from the referenced folders, you should replace the folder directory with the alias. For example, to import a component named Layout from the component folder, we replace "../../../components/Layout" with "@/components/Layout". Wow! Easy, right? Yeah. It reduces the probability of making errors during imports, especially if a project has more than one file with the same name. All you have to know is that one is in the component folder and the other is in whatever folder.

Below is a snippet that uses three (3) of the folders referenced above

```
import Layout from '@/components/Layout'
import { API_URL } from '@/config/index'
import styles from '@/styles/Form.module.css'

```

## Learn by doing.

To practice some of what was said in this article, you can try changing the import statements in the example and solution available on github [here](https://github.com/borguuh/blogs/blob/main/article-1/article-1-solution.md).

## Conclusion

Code should be made as simple as possible, just as we shouldn't need to be lifting props from 4 succesive child components by using state management tools like context API and redux, we shouldn't have to move between folders to import assets for any component moving a level up too many times.

We can surely do better.
