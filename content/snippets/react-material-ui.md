---
title: Material UI
---

## Introduction

Material-UI is an open-source project that features React components that implement Google's Material
Design ([material ui docs](https://material-ui.com/getting-started/usage/))

<br />

## Set up (includes routing)

### package.json

package.json

```json
{
  "name": "material-ui-udemy",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@material-ui/core": "^4.11.0",
    "@material-ui/icons": "^4.9.1",
    "@material-ui/styles": "^4.10.0",
    "@testing-library/jest-dom": "^4.2.4",
    "@testing-library/react": "^9.5.0",
    "@testing-library/user-event": "^7.2.1",
    "axios": "^0.19.2",
    "lodash": "^4.17.19",
    "react": "^16.13.1",
    "react-dom": "^16.13.1",
    "react-lottie": "^1.2.3",
    "react-router-dom": "^5.2.0",
    "react-scripts": "3.4.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

### App.js

App.js

```jsx
import React, { useState } from 'react';
import Header from './ui/Header'
import Footer from './ui/Footer'
import { ThemeProvider } from '@material-ui/styles'
import theme from './theme'
import { BrowserRouter, Route, Switch } from 'react-router-dom'
import LandingPage from './LandingPage'
import Services from './Services'
import CustomSoftware from './CustomSoftware'
import MobileApps from './MobileApps'
import Websites from './Websites'
import './App.css'
import Revolution from './Revolution'
import About from './About'
import Contact from './Contact'
import Estimate from './Estimate'


function App() {
  const [selectedIndex, setSelectedIndex] = useState(0)
  const [value, setValue] = useState(0)
  return (
    <ThemeProvider theme={theme}>
      <BrowserRouter>
        <Header
          value={value}
          setValue={setValue}
          selectedIndex={selectedIndex}
          setSelectedIndex={setSelectedIndex}
        />
        <Switch>
          <Route
            exact
            path="/"
            render={() => <LandingPage />}
          />
          <Route
            exact
            path="/services"
            render={() => <Services />}
          />
          <Route
            exact
            path="/customsoftware"
            render={() => <CustomSoftware
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/mobileapps"
            render={() => <MobileApps
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/websites"
            render={() => <Websites
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/revolution"
            render={() => <Revolution />}
          />
          <Route
            exact
            path="/about"
            render={() => <About
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/contact"
            render={() => <Contact
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/estimate"
            render={() => <Estimate />}
          />
        </Switch>
        <Footer
          value={value}
          setValue={setValue}
          selectedIndex={selectedIndex}
          setSelectedIndex={setSelectedIndex}
        />
      </BrowserRouter>
    </ThemeProvider>
  );
}

export default App;
```

<br />


index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

<br />

## Breakpoints

### useMediaQuery

```jsx
import React from 'react'
import Grid from '@material-ui/core/Grid'
import Typography from '@material-ui/core/Typography'
import { makeStyles, useTheme } from '@material-ui/core/styles'
import useMediaQuery from '@material-ui/core/useMediaQuery'

const useStyles = makeStyles((theme) => {
  return {
    learnButton: {
      ...theme.typography.learnButton,
      fontSize: '0.7rem',
      height: 35,
      padding: 5,
      [theme.breakpoints.down('sm')]: {
        marginBottom: '2em'
      }
    },
    background: {
      backgroundImage: `url(${background})`,
      backgroundPosition: 'center',
      backgroundSize: 'cover',
      backgroundRepeat: 'no-repeat',
      backgroundAttachment: 'fixed',
      height: '70em',
      width: '100%',
      [theme.breakpoints.down('md')]: {
        backgroundImage: `url(${mobileBackground})`,
        backgroundAttachment: 'inherit'
      }
    },
    estimateButton: {
      ...theme.typography.estimate,
      borderRadius: 50,
      height: 80,
      width: 205,
      backgroundColor: theme.palette.common.orange,
      fontSize: '1.5rem',
      [theme.breakpoints.down('sm')]: {
        marginLeft: 0,
        marginRight: 0
      }
    }
  }
})

export default function CallToAction(props) {
  const theme = useTheme()
  const matchesSM = useMediaQuery(theme.breakpoints.down('sm'))

  // const matchesMD = useMediaQuery(theme.breakpoints.down('md'))
  // const matchesSM = useMediaQuery(theme.breakpoints.down('sm'))
  // const matchesXS = useMediaQuery(theme.breakpoints.down('xs'))
  
  return (
    <Grid
      container
      style={{ height: '70em' }}
      alignItems={'center'}
      justify={matchesSM ? 'center' : 'space-between'}
      direction={matchesSM ? 'column' : 'row'}
    >
      <Grid
        item
        style={{ marginLeft: matchesSM ? 0 : '5em', textAlign: matchesSM ? 'center' : 'inherit' }}
      >
        <Grid
          container
          direction={'column'}
        >
          <Grid item>
            <Typography variant={'h2'}>
              Simple software.<br /> Revolutionary Results
            </Typography>
            <Typography>
              Take advantage of the 21th century
            </Typography>
            <Grid
              container
              item
              justify={matchesSM ? 'center' : undefined}
            >
              <Button variant={'outlined'}>
                <span style={{ marginRight: 15 }}>Learn More</span>
                <ButtonArrow
                  width={15}
                  height={15}
                />
              </Button>
            </Grid>
          </Grid>
        </Grid>
      </Grid>
      <Grid
        item
        style={{ marginRight: matchesSM ? 0 : '5em' }}
      >
        <Button variant={'contained'}>Free estimate</Button>
      </Grid>
    </Grid>
  )
}
```

<br />

### Hidden component

```jsx
import React from 'react'
import Grid from '@material-ui/core/Grid'
import Hidden from '@material-ui/core/Hidden'

export default function Footer(props) {
  return (
    <footer className={classes.footer}>

      <Hidden mdDown>
        <Grid
          container
          className={classes.mainContainer}
          justify={'center'}
        >
          <Grid
            item
            className={classes.gridItem}
          >
            <Grid
              container
              direction={'column'}
              spacing={2}
            >
              <Grid
                item
                className={classes.link}
                component={Link}
                to={'/contact'}
                onClick={() => {
                  props.setValue(4)
                }}
              >
                Contact Us
              </Grid>
            </Grid>
          </Grid>
        </Grid>
      </Hidden>

      <img
        alt={'Footer adornment'}
        src={footerAdornment}
        className={classes.adornment}
      />

      <Grid
        container
        className={classes.socialContainer}
        justify={'flex-end'}
        spacing={2}
      >
        <Grid
          item
          component={'a'}
          href={'http://www.facebook.com'}
          rel={'noopener noreferer'}
          targer={'_blank'}
        >
          <img
            alt={'facebook logo'}
            src={facebook}
            className={classes.icon}
          />
        </Grid>
        <Grid
          item
          component={'a'}
          href={'http://www.twitter.com'}
          rel={'noopener noreferer'}
          targer={'_blank'}
        >
          <img
            alt={'twitter logo'}
            src={twitter}
            className={classes.icon}
          />
        </Grid>
        <Grid
          item
          component={'a'}
          href={'http://www.instagram.com'}
          rel={'noopener noreferer'}
          targer={'_blank'}
        >
          <img
            alt={'instagram logo'}
            src={instagram}
            className={classes.icon}
          />
        </Grid>
      </Grid>
    </footer>
  )
}
```

<br />

## Scrolling

Navbar.js

```js
import React, { useEffect, useState } from 'react'
import useScrollTrigger from '@material-ui/core/useScrollTrigger';
import useMediaQuery from '@material-ui/core/useMediaQuery'


function ElevationScroll(props) {
  const { children } = props;
  const trigger = useScrollTrigger({
    disableHysteresis: true,
    threshold: 0
  });

  return React.cloneElement(children, {
    elevation: trigger ? 5 : 0,
  });
}


export default function Header(props) {

  const tabs = (
    <>
      Header
    </>
  )

  const drawer = (
    <React.Fragment>
      <SwipeableDrawer
      >
        Drawer
      </SwipeableDrawer>
    </React.Fragment>
  )

  return (
    <>
      <ElevationScroll>
        <AppBar
          position="fixed"
          className={classes.appbar}
        >
          <Toolbar disableGutters>
            <Button disableRipple component={Link} to="/" className={classes.logoContainer} onClick={() => {
              props.setValue(0)
            }}>
              <img alt={"company logo"} src={logo} className={classes.logo} />
            </Button>
            {matches ? drawer : tabs}
          </Toolbar>
        </AppBar>
      </ElevationScroll>
      <div className={classes.toolbarMargin} />
    </>
  )
}
```

<br />

## Styling

### makeStyles & useTheme

```jsx
import React from 'react'
import { Link } from 'react-router-dom'
import { makeStyles, useTheme } from '@material-ui/core/styles'
import Grid from '@material-ui/core/Grid'
import IconButton from '@material-ui/core/IconButton'
import Typography from '@material-ui/core/Typography'
import useMediaQuery from '@material-ui/core/useMediaQuery'
import Hidden from '@material-ui/core/Hidden'
import backArrow from '../assets/backArrow.svg'
import forwardArrow from '../assets/forwardArrow.svg'
import analytics from '../assets/analytics.svg'
import seo from '../assets/seo.svg'
import outreach from '../assets/outreach.svg'
import ecommerce from '../assets/ecommerce.svg'

import CallToAction from './ui/CallToAction'

const useStyles = makeStyles(theme => {
  return {
    heading: {
      maxWidth: '40em'
    },
    arrowContainer: {
      marginTop: '0.5em'
    },
    rowContainer: {
      paddingLeft: '5em',
      paddingRight: '5em',
      [theme.breakpoints.down('sm')]: {
        paddingLeft: '1.5em',
        paddingRight: '1.5em'
      }
    },
    paragraphContainer: {
      maxWidth: '30em'
    }
  }
})

export default function Websites(props) {
  const classes = useStyles()
  const theme = useTheme()
  const matchesMD = useMediaQuery(theme.breakpoints.down('md'))
  const matchesSM = useMediaQuery(theme.breakpoints.down('sm'))
  const matchesXS = useMediaQuery(theme.breakpoints.down('xs'))

  return (
    <Grid
      container
      direction={'column'}
    >

      <Grid
        item
        container
        className={classes.rowContainer}
        direction={'row'}
        justify={matchesMD ? 'center' : undefined}
        style={{ marginTop: matchesXS ? '1em' : '2em' }}
      >
        {/**CARDS WITH CONTROLS**/}
        <Hidden mdDown>
          <Grid
            item
            className={classes.arrowContainer}
            style={{ marginRight: '1em', marginLeft: '-3.5em' }}
          >
            <IconButton
              style={{ backgroundColor: 'transparent' }}
              component={Link}
              to={'/mobileapps'}
              onClick={() => {
                props.setSelectedIndex(2)
              }}
            >
              <img
                src={backArrow}
                alt={'Back to iOS/Android App development'}
              />
            </IconButton>
          </Grid>
        </Hidden>

        <Grid
          item
          container
          direction={'column'}
          className={classes.heading}
        >
          <Grid item>
            <Typography
              variant={'h2'}
              align={matchesMD ? 'center' : undefined}
            >
              Website development
            </Typography>
            <Typography
              variant={'body1'}
              paragraph
              align={matchesMD ? 'center' : undefined}
            >
              Having a website is a necessity in today’s business world. They give you one central, public location to let people know who you are, what you do, and why you’re the best at it.
            </Typography>
            <Typography
              variant={'body1'}
              paragraph
              align={matchesMD ? 'center' : undefined}
            >
              From simply having your hours posted to having a full fledged online store, making yourself as accessible as possible to users online drives growth and enables you to reach new customers.
            </Typography>
          </Grid>
        </Grid>

        <Hidden mdDown>
          <Grid
            item
            className={classes.arrowContainer}
          >
            <IconButton
              style={{ backgroundColor: 'transparent' }}
              component={Link}
              to={'/services'}
              onClick={() => props.setSelectedIndex(0)}
            >
              <img
                src={forwardArrow}
                alt={'Forward to services page'}
              />
            </IconButton>
          </Grid>
        </Hidden>
      </Grid>

      <Grid
        item
        container
        direction={matchesSM ? 'column' : 'row'}
        alignItems={'center'}
        className={classes.rowContainer}
        style={{ marginTop: '15em' }}
      >
        <Grid item>
          <Grid
            container
            direction={'column'}
          >
            <Grid item>
              <Typography
                align={matchesSM ? 'center' : undefined}
                variant={'h4'}
                gutterBottom
              >
                Analytics
              </Typography>
            </Grid>
            <Grid item>
              <img
                src={analytics}
                style={{ marginLeft: '-2.75em' }}
                alt={'graph with magnifying glass revealing 1\'s and 0\'s}'}
              />
            </Grid>
          </Grid>
        </Grid>
        <Grid
          item
          className={classes.paragraphContainer}
        >
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            Knowledge is power, and data is 21st Century gold. Analyzing this data can reveal hidden patterns and trends in your business, empowering you to make smarter decisions with measurable effects.
          </Typography>
        </Grid>
      </Grid>

      <Grid
        item
        container
        direction={matchesSM ? 'column' : 'row'}
        justify={'flex-end'}
        alignItems={'center'}
        className={classes.rowContainer}
        style={{ marginTop: '15em', marginBottom: '15em' }}
      >
        <Grid item>
          <Grid
            container
            direction={'column'}
          >
            <Grid item>
              <Typography
                variant={'h4'}
                align={'center'}
                gutterBottom
              >
                E-Commerce
              </Typography>
            </Grid>
            <Grid item>
              <img
                src={ecommerce}
                alt={'World outline made of dollar signs'}
              />
            </Grid>
          </Grid>
        </Grid>
        <Grid
          item
          style={{ marginLeft: matchesSM ? 0 : '1em' }}
          className={classes.paragraphContainer}
        >
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            It’s no secret that people like to shop online.
          </Typography>
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            In 2017 over $2.3 trillion was spent in e-commerce, and it’s time for your slice of that pie.
          </Typography>
        </Grid>
      </Grid>

      <Grid
        item
        container
        direction={matchesSM ? 'column' : 'row'}
        alignItems={'center'}
        className={classes.rowContainer}
        style={{ marginBottom: '15em' }}
      >
        <Grid item>
          <Grid
            container
            direction={'column'}
          >
            <Grid item>
              <Typography
                align={matchesSM ? 'center' : undefined}
                variant={'h4'}
                gutterBottom
              >
                Outreach
              </Typography>
            </Grid>
            <Grid item>
              <img
                src={outreach}
                alt={'megaphone'}
              />
            </Grid>
          </Grid>
        </Grid>
        <Grid
          item
          style={{ marginLeft: matchesSM ? 0 : '1em' }}
          className={classes.paragraphContainer}
        >
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            Draw people in with a dazzling website. Showing off your products online is a great way to help customers decide what’s right for them before visiting in person.
          </Typography>
        </Grid>
      </Grid>

      <Grid
        item
        container
        direction={matchesSM ? 'column' : 'row'}
        justify={'flex-end'}
        alignItems={'center'}
        className={classes.rowContainer}
        style={{ marginBottom: '15em' }}
      >
        <Grid item>
          <Grid
            container
            direction={'column'}
          >
            <Grid item>
              <Typography
                variant={'h4'}
                align={'center'}
                gutterBottom
              >
                Search Engine <br />Optimization
              </Typography>
            </Grid>
            <Grid item>
              <img
                src={seo}
                style={{ marginLeft: '-2.75em' }}
                alt={'Website standing on winner\'s podium}'}
              />
            </Grid>
          </Grid>
        </Grid>
        <Grid
          item
          style={{ marginLeft: '1em' }}
          className={classes.paragraphContainer}
        >
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            How often have you ever been to the second page of Google results?
          </Typography>
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            If you’re like us, probably never.
          </Typography>
          <Typography
            align={matchesSM ? 'center' : undefined}
            variant={'body1'}
            paragraph
          >
            Customers don’t go there either, so we make sure your website is designed to end up on top.
          </Typography>
        </Grid>
      </Grid>

      <Grid>
        <CallToAction setValue={props.setValue} />
      </Grid>

    </Grid>
  )
}
```

<br />

### Theme

App.js

````jsx
import React, { useState } from 'react';
import Header from './ui/Header'
import Footer from './ui/Footer'
import { ThemeProvider } from '@material-ui/styles'
import theme from './theme'
import { BrowserRouter, Route, Switch } from 'react-router-dom'
import LandingPage from './LandingPage'
import Services from './Services'
import CustomSoftware from './CustomSoftware'
import MobileApps from './MobileApps'
import Websites from './Websites'
import './App.css'
import Revolution from './Revolution'
import About from './About'
import Contact from './Contact'
import Estimate from './Estimate'


function App() {
  const [selectedIndex, setSelectedIndex] = useState(0)
  const [value, setValue] = useState(0)
  return (
    <ThemeProvider theme={theme}>
      <BrowserRouter>
        <Header
          value={value}
          setValue={setValue}
          selectedIndex={selectedIndex}
          setSelectedIndex={setSelectedIndex}
        />
        <Switch>
          <Route
            exact
            path="/"
            render={() => <LandingPage />}
          />
          <Route
            exact
            path="/services"
            render={() => <Services />}
          />
          <Route
            exact
            path="/customsoftware"
            render={() => <CustomSoftware
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/mobileapps"
            render={() => <MobileApps
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/websites"
            render={() => <Websites
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/revolution"
            render={() => <Revolution />}
          />
          <Route
            exact
            path="/about"
            render={() => <About
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/contact"
            render={() => <Contact
              setSelectedIndex={setSelectedIndex}
              setValue={setValue}
            />}
          />
          <Route
            exact
            path="/estimate"
            render={() => <Estimate />}
          />
        </Switch>
        <Footer
          value={value}
          setValue={setValue}
          selectedIndex={selectedIndex}
          setSelectedIndex={setSelectedIndex}
        />
      </BrowserRouter>
    </ThemeProvider>
  );
}

export default App;
````

<br />

theme.js

```javascript
import { createMuiTheme } from '@material-ui/core/styles'

const arcBlue = '#0b72b9'
const arcOrange = '#ffba60'
const arcGrey = '#868686'

const theme = createMuiTheme({
  palette: {
    common: {
      blue: `${arcBlue}`,
      orange: `${arcOrange}`
    },
    primary: {
      main: `${arcBlue}`
    },
    secondary: {
      main: `${arcOrange}`
    }
  },
  typography: {
    tab: {
      fontFamily: 'Raleway',
      textTransform: 'none',
      fontWeight: 700,
      fontSize: '1rem'
    },
    estimate: {
      fontFamily: 'Pacifico',
      fontSize: '1rem',
      textTransform: 'none',
      color: 'white'
    },
    h2: {
      fontFamily: 'Raleway',
      fontWeight: 700,
      fontSize: '2.5rem',
      color: `${arcBlue}`,
      lineHeight: 1.5
    },
    h3: {
      fontFamily: 'Pacifico',
      fontSize: '2.5rem',
      color: `${arcBlue}`
    },
    h4: {
      fontFamily: 'Raleway',
      fontSize: '1.75rem',
      color: `${arcBlue}`,
      fontWeight: 700
    },
    h6: {
      fontWeight: 500,
      fontFamily: 'Raleway',
      color: arcBlue,
      lineHeight: 1
    },
    subtitle1: {
      fontSize: '1.25rem',
      fontWeight: 300,
      color: `${arcGrey}`
    },
    subtitle2: {
      color: 'white',
      fontSize: '1.25rem',
      fontWeight: 300
    },
    body1: {
      fontSize: '1.25rem',
      color: arcGrey,
      fontWeight: 300
    },
    learnButton: {
      borderColor: `${arcBlue}`,
      color: `${arcBlue}`,
      borderWidth: 2,
      textTransform: 'none',
      borderRadius: 50,
      fontFamily: 'Roboto',
      fontWeight: 'bold',
    },
    caption: {
      fontSize: '1rem',
      fontWeight: 300,
      color: arcGrey
    }
  },
  overrides: {
    MuiInputLabel: {
      root: {
        color: arcBlue,
        fontSize: '1rem'
      }
    },
    MuiInput: {
      root: {
        color: arcGrey,
        fontWeight: 300
      },
      underline: {
        '&:before': {
          borderBottom: `2px solid ${arcBlue}`
        },
        '&:hover:not($disabled):not($focused):not($error):before': {
          borderBottom: `2px solid ${arcBlue}`
        }
      }
    }
  }
})

export default theme
```

<br />

### Typography

```jsx
<Typography
  variant={'h2'}
  align={matchesMD ? 'center' : undefined}
>
  About Us
</Typography>

<Typography variant={'caption'}>a page from my Sophmore yearbook</Typography>


<Typography
  variant={'body1'}
  paragraph
  gutterBottom
  align={'center'}
>
  I started coding when I was 9 years old.
</Typography>
```

<br />

## Layout

### Grid container

```jsx
<Grid
  container
  direction={'column'}
  justify={'center'}
  alignItems={'center'}
  style={{
    marginBottom: matchesSM ? '1em' : matchesMD ? '5em' : 0,
    marginTop: matchesSM ? '1em' : matchesMD ? '5em' : 0
  }}
>
  <Grid item>Item</Grid>
  <Grid item>Item</Grid>
  <Grid item>Item</Grid>
</Grid>

<Grid
  container
  direction={'row'}
>
  <Grid item>Item</Grid>
  <Grid item>Item</Grid>
  <Grid item>Item</Grid>
</Grid>
```

<br />

### Grid item

```jsx
<Grid
  item
  xs={2}
>
  <img
    src={check}
    alt={'checkmark'}
  />
</Grid>

<Grid
  item
  lg
>
</Grid>
```

<br />

### Advanced grid example

```jsx
<Grid
  item
  container
  direction={matchedMD ? 'column' : 'row'}
  alignItems={matchedMD ? 'center' : undefined}
  justify={'space-around'}
  style={{ marginBottom: '10em' }}
  className={classes.rowContainer}
>
  <Grid
    item
    container
    direction={matchesSM ? 'column' : 'row'}
    className={classes.itemContainer}
    md
    style={{ marginBottom: matchedMD ? '15em' : 0 }}
  >

    <Grid
      item
      container
      direction={'column'}
      md
    >
      <Grid item>
        <Typography
          variant={'h4'}
          align={matchesSM ? 'center' : undefined}
        >Digital documents & data</Typography>
      </Grid>
      <Grid item>
        <Typography
          variant={'body1'}
          paragraph
          align={matchesSM ? 'center' : undefined}
        >
          Reduce Errors. Reduce Waste. Reduce Costs.
        </Typography>
        <Typography
          variant={'body1'}
          paragraph
          align={matchesSM ? 'center' : undefined}
        >
          Billions are spent annually on the purchasing, printing, and distribution of paper. On top of the massive environmental impact this has, it causes harm to your bottom line as well.
        </Typography>
        <Typography
          variant={'body1'}
          paragraph
          align={matchesSM ? 'center' : undefined}
        >
          By utilizing digital forms and documents you can remove these obsolete expenses, accelerate your communication, and help the Earth.
        </Typography>
      </Grid>
    </Grid>

    <Grid
      item
      md
    >
      <Lottie
        options={documentsOptions}
        isStopped={true}
        style={{ maxHeight: 265, maxWidth: 275, minHeight: 250 }}
      />
    </Grid>

  </Grid>
  <Grid
    item
    container
    direction={matchesSM ? 'column' : 'row'}
    className={classes.itemContainer}
    md
  >

    <Grid
      item
      md
    >
      <Lottie
        options={scaleOptions}
        isStopped={true}
        style={{ maxHeight: 325, maxWidth: 280, minHeight: 260 }}
      />
    </Grid>

    <Grid
      item
      container
      direction={'column'}
      md
    >
      <Grid item>
        <Typography
          variant={'h4'}
          align={matchesSM ? 'center' : 'right'}
        >Scale</Typography>
      </Grid>
      <Grid item>
        <Typography
          variant={'body1'}
          align={matchesSM ? 'center' : 'right'}
          paragraph
        >
          Whether you’re a large brand, just getting started, or 
          taking off right now, our application architecture ensures 
          pain-free growth and reliability.
        </Typography>
      </Grid>
    </Grid>

  </Grid>
</Grid>
```

<br />

### Card

```jsx
import Card from '@material-ui/core/Card'
import CardContent from '@material-ui/core/CardContent'
import {makeStyles, useTheme} from '@material-ui/core/styles'
import Grid from '@material-ui/core/Grid'

const useStyles = makeStyles(theme => {
  return {
    revolutionBackground: {
      backgroundImage: `url(${revolutionBackground})`,
      backgroundPosition: 'center',
      backgroundSize: 'cover',
      backgroundRepeat: 'no-repeat',
      height: '100%',
      width: '100%'
    },
    revolutionCard: {
      position: 'absolute',
      boxShadow: theme.shadows[10],
      borderRadius: 15,
      padding: '10em',
      [theme.breakpoints.down('sm')]: {
        paddingTop: '8em',
        paddingBottom: '8em',
        paddingLeft: 0,
        paddingRight: 0,
        borderRadius: 0,
        width: '100%'
      }
    },
  }
})

export default function CardExample() {
  return <Card className={classes.revolutionCard}>
      <CardContent>
        <Grid container direction={'column'} style={{textAlign: 'center'}}>
          <Grid item>
            <Typography variant={'h3'} gutterBottom>
              The Revolution
            </Typography>
          </Grid>
          <Grid item>
            <Typography variant={'subtitle1'}>
              Visionary insights coupled with cutting-edge is a recipe for revolution
            </Typography>
            <Button variant={'outlined'} className={classes.learnButton}>
              <span style={{marginRight: 10}}>Learn More</span>
              <ButtonArrow width={10} height={10} fill={theme.palette.common.blue}/>
            </Button>
          </Grid>
        </Grid>
      </CardContent>
    </Card>
}
```

## Forms

### Text Field

```jsx
export default function Contact() {
  const [name, setName] = useState('')
  const [email, setEmail] = useState('')
  const [emailHelper, setEmailHelper] = useState('')

  const onChange = event => {
    let valid;

    switch (event.target.id) {
      case 'email':
        setEmail(event.target.value)
        valid = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(event.target.value)
        if (!valid) {
          setEmailHelper('invalid email')
        } else {
          setEmailHelper('')
        }
        break
      default:
        break
    }
  }

  return (
    <>
      <TextField
        label={'Name'}
        id={'name'}
        value={name}
        style={{ marginBottom: '0.5em' }}
        fullWidth
        onChange={(e) => {
          setName(e.target.value)
        }}
      />

      <TextField
        label={'Email'}
        id={'email'}
        value={email}
        error={emailHelper.length !== 0}
        helperText={emailHelper}
        style={{ marginBottom: '0.5em' }}
        fullWidth
        onChange={(e) => {
          onChange(e)
        }}
      />
    </>
  )
}
```

<br />

### Button

```jsx
<Button
  variant={'container'}
  className={classes.estimateButton}
  onClick={() => {
    setDialogOpen(true);
    getTotal()
    getPlatforms()
    getFeatures()
    getCustomFeatures()
    getCategory()
  }}
>
  Get estimate
</Button>
```

<br />

### Icon buttons 

```jsx
<IconButton
  style={{backgroundColor: 'transparent'}}
  component={Link}
  to={'/mobileapps'}
  onClick={() => props.setSelectedIndex(2)}
>
  <img src={forwardArrow} alt={'Forward to iOS/Android App Development Page'}/>
</IconButton>
```

<br />

### Button with link

```jsx
import {Link} from 'react-router-dom'

export default function ButtonWithLink () {
  return (
    <IconButton
      style={{backgroundColor: 'transparent'}}
      component={Link}
      to={'/mobileapps'}
      onClick={() => props.setSelectedIndex(2)}
    >
      <img src={forwardArrow} alt={'Forward to iOS/Android App Development Page'}/>
    </IconButton>
  )
}
```

<br />

## Dialog

Dialog.js

```jsx
import { makeStyles, useTheme } from '@material-ui/core/styles'
import Grid from '@material-ui/core/Grid'
import Typography from '@material-ui/core/Typography'
import Button from '@material-ui/core/Button'
import TextField from '@material-ui/core/TextField'

import Dialog from '@material-ui/core/Dialog'
import DialogContent from '@material-ui/core/DialogContent'

export default function Contact() {
  const [open, setOpen] = useState(false)

  const theme = useTheme()
  const matchesSM = useMediaQuery(theme.breakpoints.down('sm'))

  const onConfirm = () => {
    setLoading(true)
    axios.get('https://jsonplaceholder.typicode.com/posts')
      .then(result => {
        setName('')
        setEmail('')
        setPhone('')
        setMessage('')
        setAlert({ open: true, message: 'Message sent succesfully', backgroundColor: '#4BB543' })
      })
      .then(() => new Promise(resolve => setTimeout(resolve, 5000)))
      .catch(e => {
        setAlert({ open: true, message: 'Something went wrong', backgroundColor: '#ff3232' })
      })
      .finally(() => {
        setOpen(false)
        setLoading(false)
      })
  }

  return (
    <>
      <Dialog
        fullScreen={matchesSM}
        open={open}
        fullWidth
        onClose={() => {
          setOpen(false)
        }}
        PaperProps={{
          style: {
            paddingTop: matchesXS ? '1em' : '5em',
            paddingBottom: matchesXS ? '1em' : '5em',
            paddingLeft: matchesXS ? 0 : matchesSM ? '5em' : matchesMD ? '15em' : '25em',
            paddingRight: matchesXS ? 0 : matchesSM ? '5em' : matchesMD ? '15em' : '25em'
          }
        }}
        style={{ zIndex: 1302 }}
      >
        <DialogContent>
          <Grid
            container
            direction={'column'}
          >
            <Grid item>
              <Typography
                gutterBottom
                variant={'h4'}
                align={'center'}
              >
                Confirm Message
              </Typography>
            </Grid>
            <Grid item>
              <TextField
                label={'Name'}
                id={'name'}
                value={name}
                style={{ marginBottom: '0.5em' }}
                fullWidth
                onChange={(e) => {
                  setName(e.target.value)
                }}
              />
            </Grid>
            <Grid
              item
              container
              style={{ marginTop: '2em' }}
              alignItems={'center'}
              direction={matchesSM ? 'column' : 'row'}
            >
              <Grid item>
                <Button
                  color={'primary'}
                  onClick={() => setOpen(false)}
                  sytle={{ fontWeight: 300 }}
                >
                  Cancel
                </Button>
              </Grid>
              <Grid item>
                <Button
                  variant={'contained'}
                  className={classes.sendButton}
                  onClick={() => onConfirm()}
                  disabled={name.length === 0 || emailHelper.length !== 0 || phoneHelper.length !== 0 || message.length === 0 || email.length === 0 || phone.length === 0}
                >
                  {loading ? <CircularProgress size={30} /> : buttonContents}
                </Button>
              </Grid>
            </Grid>
          </Grid>
        </DialogContent>
      </Dialog>
    </>
  )
}
```

<br />

## Loading

### LoadingButton

```jsx
import React, { useState } from 'react'
import CircularProgress from '@material-ui/core/CircularProgress'
import axios from 'axios'

export default function LoadingButton() {
  const [loading, setLoading] = useState(false)

  const onConfirm = () => {
    setLoading(true)
    axios.get('https://jsonplaceholder.typicode.com/posts')
      .then(result => {
        setName('')
        setEmail('')
        setPhone('')
        setMessage('')
        setAlert({ open: true, message: 'Message sent succesfully', backgroundColor: '#4BB543' })
      })
      .then(() => new Promise(resolve => setTimeout(resolve, 5000)))
      .catch(e => {
        setAlert({ open: true, message: 'Something went wrong', backgroundColor: '#ff3232' })
      })
      .finally(() => {
        setOpen(false)
        setLoading(false)
      })
  }

  return (
    <>
      <Grid
        item
        container
        style={{ marginTop: '2em' }}
        alignItems={'center'}
        direction={matchesSM ? 'column' : 'row'}
      >
        <Grid item>
          <Button
            color={'primary'}
            onClick={() => setOpen(false)}
            sytle={{ fontWeight: 300 }}
          >
            Cancel
          </Button>
        </Grid>
        <Grid item>
          <Button
            variant={'contained'}
            className={classes.sendButton}
            onClick={() => onConfirm()}
            disabled={name.length === 0 || emailHelper.length !== 0 || phoneHelper.length !== 0 || message.length === 0 || email.length === 0 || phone.length === 0}
          >
            {loading ? <CircularProgress size={30} /> : buttonContents}
          </Button>
        </Grid>
      </Grid>
    </>

  )
}

```

<br />

### CircularProgress/Loader

```jsx
import CircularProgress from '@material-ui/core/CircularProgress'

export default function CircularProgressExample() {
  return <>
    <CircularProgress size={30} />
  </>
}
```

<br />

## Snackbar/Toaster

```jsx
import Snackbar from '@material-ui/core/Snackbar'
import Button from '@material-ui/core/Button'
import CircularProgress from '@material-ui/core/CircularProgress'
import axios from 'axios'

export default function SnackbarExample() {

  const [alert, setAlert] = useState({
    open: false,
    message: '',
    backgroundColor: ''
  })

  const onConfirm = () => {
    setLoading(true)
    axios.get('https://jsonplaceholder.typicode.com/posts')
      .then(result => {
        setName('')
        setEmail('')
        setPhone('')
        setMessage('')
        setAlert({ open: true, message: 'Message sent succesfully', backgroundColor: '#4BB543' })
      })
      .then(() => new Promise(resolve => setTimeout(resolve, 5000)))
      .catch(e => {
        setAlert({ open: true, message: 'Something went wrong', backgroundColor: '#ff3232' })
      })
      .finally(() => {
        setOpen(false)
        setLoading(false)
      })
  }

  return (
    <>
      <Snackbar
        open={alert.open}
        message={alert.message}
        ContentProps={{ style: { backgroundColor: alert.backgroundColor } }}
        anchorOrigin={{ vertical: 'top', horizontal: 'center' }}
        onClose={() => {
          setAlert({ ...alert, open: false })
        }}
        autoHideDuration={4000}
      />
      <Button
        variant={'contained'}
        className={classes.sendButton}
        onClick={() => onConfirm()}
        disabled={name.length === 0 || emailHelper.length !== 0 || phoneHelper.length !== 0 || message.length === 0 || email.length === 0 || phone.length === 0}
      >
        {loading ? <CircularProgress size={30} /> : buttonContents}
      </Button>
    </>
  )
}
```

<br />

