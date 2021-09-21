---
title: "Example"
date: 2021-09-21T18:28:57+01:00
tags: ["example"]
draft: false
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum sapien odio, sodales ac erat ut, commodo pulvinar velit. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Nulla quis risus id justo pulvinar faucibus. Integer scelerisque auctor libero id aliquet. Nunc vitae odio congue, posuere libero at, hendrerit ex. Curabitur bibendum pulvinar felis non ullamcorper. Quisque tristique justo eu libero tristique fringilla. Sed venenatis non lorem non vestibulum. Vivamus vel porttitor elit.

```go
package fizz_buzz

import (
	"strconv"
)

func Convert(number int) string {
	if isDivisibleBy(number, 15) {
		return "FizzBuzz"
	}

	if isDivisibleBy(number, 3) {
		return "Fizz"
	}

	if isDivisibleBy(number, 5) {
		return "Buzz"
	}

	return strconv.Itoa(number)
}

func isDivisibleBy(number int, factor int) bool {
	return number%factor == 0
}
```

Cras eget ex nisl. Praesent tincidunt congue ligula, eu aliquet massa dignissim eget. Interdum et malesuada fames ac ante ipsum primis in faucibus. Suspendisse ut elementum metus. Vivamus tempor nibh nec ante pharetra congue. Donec nec purus neque. Aenean quis scelerisque arcu, vel porta tortor. Cras non tincidunt turpis, at pharetra tortor.

Aliquam porttitor nulla odio, facilisis ornare orci egestas et. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Vivamus dictum vitae felis nec rhoncus. Cras pharetra turpis et mi fringilla, non posuere lorem aliquam. Nulla facilisi. Etiam eget augue in enim scelerisque cursus. Maecenas nec porttitor nisi. Aenean tortor mauris, finibus sed turpis id, ultrices sagittis neque. Vestibulum a sodales nisi. Vivamus cursus vestibulum lectus, non maximus libero rutrum vel. Vivamus vel velit neque. Duis est odio, consequat et auctor quis, ullamcorper sed metus. Sed ac libero eu ligula luctus molestie. Nam congue, augue facilisis efficitur dignissim, sem arcu mollis urna, non semper ligula nunc id lorem. Sed elit lacus, ullamcorper at velit a, commodo iaculis felis. Aenean vel hendrerit massa.
