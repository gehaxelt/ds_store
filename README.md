Go .DS_Store parser
==================

This is a quick and dirty Go parser for `.DS_Store` files. I focused on the extraction of file names and ignored other parameters.

It does not support the full set of structure formats, so it might break in some scenarios.

#Useful information:
The following descriptions and sourcecodes were quite helpful:
- <http://search.cpan.org/~wiml/Mac-Finder-DSStore/DSStoreFormat.pod>
- <https://wiki.mozilla.org/DS_Store_File_Format> 

#Example code

```
package main

import (
	"flag"
	"fmt"
	"io/ioutil"
	"os"

	"github.com/gehaxelt/ds_store"
)

func main() {

	var fileName string

	flag.StringVar(&fileName, "i", "", "DS_Store input file")
	flag.Parse()

	if fileName == "" {
		if len(os.Args) >= 2 {
			fileName = os.Args[1]
		} else {
			fileName = ".DS_Store"
		}
	}

	if _, err := os.Stat(fileName); os.IsNotExist(err) {
		panic("No file given or it does not exist:" + fileName)
	}

	dat, err := ioutil.ReadFile(fileName)

	if err != nil {
		panic(err)
	}

	a, err := ds_store.NewAllocator(dat)

	filenames, err := a.TraverseFromRootNode()
	if err != nil {
		panic(err)
	}

	for _, f := range filenames {
		fmt.Printf("File: %s \n", f)
	}
}
```

```
./ds_storeviewer -i samples/DS_Store-2
File: joomlatools-files 
File: joomlatools-files 
File: joomlatools-files 
File: joomlatools-files 
```

# License
See License.md