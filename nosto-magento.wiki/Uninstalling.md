There are multiple ways of uninstalling the extension and the correct method depends upon how the extension was installed.

### Uninstalling via the command-line (CLI)

In some cases, it may be possible to uninstall the extension using the `mage` CLI.

```
./mage uninstall community Nosto_Tagging
```

**Note::** Even though your package is installed, the Magento CLI might fail to be able to remove it, in which case you will see an error "uninstall: Package is not installed". In this case, you will need to manually remove the extension.

## Uninstalling via Modman

If you've installed the extension using Modman, it is recommended that you uninstall it via Modman.

```shell
modman undeploy nosto-magento
modman remove nosto-magento
```

## Uninstalling via Magento Connect Manager

If your package was installed via Magento Connect Manager, you can uninstall it via Magento Connect Manager. 

Open the Magento Connect Manager by navigating to "System" > "Magento Connect" > "Magento Connect Manager". Locate the section in the Magento Connect Manager labeled "Manage Existing Extensions". Find the Nosto entry and from the drop-down, select "Uninstall" and then click "Commit Changes".