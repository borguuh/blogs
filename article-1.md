Is your code DRY or WET? I'm not talking about water in this case. Well, DRY here means Don't Repeat Yourself and WET means Write Everything Twice. You surely don't want to be from those who do the latter. I mean, why write it multiple times when you can write it once?

I was recently added to a team project which was built in react.js. While I was goint through the folders and files. I found one thing that gave a little awkward feeling. Almost all the files had to import assets like css, images, logos, svg, icons, fonts etc - which is normal - but the awkward thing is the repetitive way in which files are being imported. Below is an excerpt from a file in the project. 

NB : Notice the `from "../../components", "../../assets/icons", "../../assets/images"` etc

```
import React from 'react';
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

You might be wondering, why do you have to repeatedly type out the same boring codes so many times? Well, you should. But the goodnews is, you don't have to go through all these, because the jsconfig.json file has got your back.

[image] hug image or icon for GOTCHA!
 
# jsconfig.json

The jsconfig.json is a file that holds the configuration settings for the js files in the project structure. It basicaly dictates or overrides how javascript should be treated for the project in question. This is similar to the tsconfig.json for typescript developers. It is automatically recognized as the configuration file once it can be found in the root folder. We'll be using this for those who aren't typescript devs who use the tsconfig.json file

So, rather than to manually import each module, image etc by going two or more level up or down to get the specific file or function, jsconfig.json does the heavy lifting for us.


# Creating module aliases with jsconfig.json

It is as simple as adding a new jsconfig.json file in the root folder. Thereafter, create a json object in it with "compilerOptions" key. Then specify the project base url with the "baseUrl" option, then specify the various paths to each file you want to reference with the "paths" option while using the reference key value pair as shown below. 

NB : There are many configuration options that can be applied to projects but the one that suits the goal here is the "compilerOptions" key.

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

In the example above, there five (5) folders named components, context, config, styles and helpers, each occupying the value space. While the reference key to call each folder is the alias in each case. So, importing from the "@/components" will automatically go to the components folder wherever you are in the file tree.

# Calling the reference folders by their alias

To import anything from the referenced folders, all that has to be done is to replace the respective normal folder directory with the alias. For example, to import a component named Layout from the component folder, we replace "../../../components/Layout" with "@/components/Layout". Wow! Easy, right? Yeah. It reduces the probability of making errors during imports, especially if a project has more than one file with the same name. All you have to know is that one is in the component folder and the other is in whatever folder.

Below is a snippet that uses three (3) of the folders referenced above

```
import Layout from '@/components/Layout'
import { API_URL } from '@/config/index'
import styles from '@/styles/Form.module.css'

```

# Example

Let's say you want write a javascript functional component that looks like the one below. And you want your code to be of the easier and lesser format explained above. assuming that the src folder is in the root folder

```
import React from "react";
import styled from "styled-components";
import { ReactComponent as PrimaryLogo } from '../assets/images/logos/primary.svg'
import { ReactComponent as PrimaryLogoText } from '../assets/images/logos/primary_logo_text.svg'

const Logo = () => {
    return (
        <div className="flex align-center">
            <LogoContainer>
                <PrimaryLogo />
            </LogoContainer>
            <PrimaryLogoText />
        </div>
    )
}

export default Logo
```

Part 1

All you have to do is to write this in the jsconfig.json file in the root folder.

```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/logos/*": ["src/assets/images/logos/*"]
    }
  }
}

```
Part 2

Then import files as written below

```
import React from "react";
import styled from "styled-components";
import { ReactComponent as PrimaryLogo } from '@/logos/primary.svg'
import { ReactComponent as PrimaryLogoText } from '@/logos/primary_logo_text.svg'

```
So easy

# Learn by doing.

To practice a little of what was said in this article, you can try changing the import statements below. The project's root folder has a "src" folder which contains the folders housing the files included in the import. You can infer the folder structure from the import statements. You can view the solution in the github link below.
[link ]

```
import React from "react";
import styled from "styled-components";
import { COLORS, margin } from "../../assets/styles/styles";
import Grid from "../../components/Grid";
import { H1, H3, P } from "../../components/Texts";
import FloatingStar from "../../components/FloatingStar";
import MainWrapper from "../../components/MainWrapper";
import { ReactComponent as DiagonalArrow } from '../../assets/icons/DiagonalArrow.svg'
import faces from '../../assets/images/faces.png'
import FooterLinks from "../../components/FooterLinks";

```

```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/components/*": ["src/components/*"],
      "@/styles/*": ["src/assets/styles/*"],
	  "@/icons/*": ["src/assets/icons/*"],
	  "@/images/*": ["src/assets/images/*"],
    }
  }
}

```

```
import React from "react";
import styled from "styled-components";
import { COLORS, margin } from "@/styles/styles";
import Grid from "@/components/Grid";
import { H1, H3, P } from "@/components/Texts";
import FloatingStar from "@/components/FloatingStar";
import MainWrapper from "@/components/MainWrapper";
import { ReactComponent as DiagonalArrow } from '@/icons/DiagonalArrow.svg'
import faces from '@/images/faces.png'
import FooterLinks from "@/components/FooterLinks";

```



# Conclusion

Code should be made as simple as possible, just as we shouldn't need to be lifting props from 4 succesive child components by using state management tools like context API and redux, we shouldn't have to move between folders to import assets for any component moving a level up too many times.

We can surely do better.