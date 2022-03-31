# Solution to the excercise

Below is the part by part solution to the practice excercise at the end of my blog post available at [link]


## Part 1

For the jsconfig.json file

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

## Part 2

For the sample file and any other file in the project structure

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
