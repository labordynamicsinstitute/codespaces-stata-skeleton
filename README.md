# Devcontainer skeleton with Stata only

This is set up for standard Codespaces **command line** operation, and optimized for AEA data editorial activities. It can be used for general Stata processing as well.

## TL;DR

- Enable [Codespaces for your Github account/organization](https://docs.github.com/en/codespaces/)
- Create a "Codespace secret":
  - STATA_LIC_BASE64 - base64 encoded version of the Stata license file, see [this discussion](https://github.com/AEADataEditor/stata-project-with-docker). It can be generated with this single command line: `gh secret set STATA_LIC_BASE64 -b"$(cat stata.lic | base64)" -v all -o YOURORG` from within Codespaces after a one-time upload of the stata.lic file. You can use the Stata license installed on your own PC or Mac.
- Go to your personal [Codespaces](https://github.com/codespaces) space, and search for this repository to [start a codespace](https://docs.github.com/en/codespaces/developing-in-codespaces/opening-an-existing-codespace), or, alternatively, fork this repository into your Github organization and launch Codespaces from the green button menu.

## More info

For more information about required setup (in particular how to use Stata in this image), see [https://github.com/labordynamicsinstitute/codespaces-stata-r-skeleton](https://github.com/labordynamicsinstitute/codespaces-stata-r-skeleton).
