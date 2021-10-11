# Personal Website using the Hugo Academic Theme

## Steps to Build/Update
* Download [hugo extended](https://github.com/gohugoio/hugo/releases)
  * Be sure to select the "extended" version
  * Version 0.63.1 is known to work with the current theme here
* To test the site, cd to its root directory and run `<path_to_hugo_extended>/hugo.exe server -F`
  * This will render the site locally
  * `-F` builds content with a future date (useful for publications that are _to appear_)
* Once satisfied, run `<path_to_hugo_extended>/hugo.exe -F`
* This will place rendered content into a directory named `public`
* You can setup GitHub hosting for Hugo ([steps here](https://gohugo.io/hosting-and-deployment/hosting-on-github/)) and push the rendered content to your GitHub repo
