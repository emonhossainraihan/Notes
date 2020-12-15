https://advancedweb.hu/what-is-the-async-disposer-pattern-in-javascript/

## Problem

```js
const browser = await puppeteer.launch({/* ... */});
try {
	const page = await browser.newPage();
	// ...
	const screenshot = await page.screenshot({/* ... */});

	// the browser can be closed but how to use the screenshot outside the try..finally?
} finally {
	await browser.close();
}
```

## A suboptimal solution is to declare a variable outside the block and use it to store the image:

```js
let screenshot;
const browser = await puppeteer.launch({/* ... */});
try {
	const page = await browser.newPage();
	// ...
	screenshot = await page.screenshot({/* ... */});
} finally {
	await browser.close();
}
// use screenshot
```

## A better solution is to use a function:

```js
const makeScreenshot = async () => {
	const browser = await puppeteer.launch({/* ... */});
	try {
		const page = await browser.newPage();
		// ...
		return await page.screenshot({/* ... */});
	} finally {
		await browser.close();
	}
}

const screenshot = await makeScreenshot();
```

## The withBrowser function contains the logic to launch and close the browser, and the fn function gets and uses the browser instance.


```js
const withBrowser = async (fn) => {
	const browser = await puppeteer.launch({/* ... */});
	try {
		return await fn(browser);
	} finally {
		await browser.close();
	}
}

const screenshot = await withBrowser(async (browser) => {
	const page = await browser.newPage();
	// ...
	return await page.screenshot({/* ... */});
});
```
