# PHIVE Install Action
PHIVE Install Action is a GitHub Action to download [PHIVE](https://phar.io/), install tools using the `phars.xml` file in your repository, and upload them to the artifact store.

## Requirements
PHIVE Install Action has the following requirements:

* PHP

## Usage
Use PHIVE Install Action as a step in the job after the checkout step of your repository, as follows:
```yaml
- name: Install tools
  uses: ngmy/phive-install-action@master
```

You can also upload tools to the artifact store for use in subsequent jobs, as follows:
```yaml
jobs:
  install_tools:
    name: Install tools
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v2
      - name: Install tools
        uses: ngmy/phive-install-action@master
      - name: Upload tools to artifact store
        uses: actions/upload-artifact@master
        with:
          name: tools
          path: tools
  test:
    name: Test
    runs-on: ubuntu-latest
    needs: install_tools
    steps:
      - name: Check out repository
        uses: actions/checkout@v2
      - name: Download tools from artifact store
        uses: actions/download-artifact@master
        with:
          name: tools
          path: tools
      - name: Set tools as an executable
        run: find tools -type f -print0 | xargs -0 chmod +x
```

## License
PHIVE Install Action is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).
