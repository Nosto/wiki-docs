# Setting up the IDE

{% hint style="warning" %}
You are reading the NextGen Documentation

NextGen widgets are a new and improved way to display UGC content.&#x20;

On Oct 1st, all widgets created will be NextGen.

Please check your widget version on the Widget List page to see if it is **Classic** or **NextGen** widget.

You can read the [Classic Widget Documentation](../../../../onsite-widgets/) here.

**Note: This feature is unique to NextGen widgets**
{% endhint %}

Once you have read our guide on [creating-a-development-space-for-your-team.md](creating-a-development-space-for-your-team.md "mention"), you can begin setting up your IDE for widget template development.

To get started, follow the instructions below

1\) Clone your Forked repo of stackla/stackla-widget-templates. You can do this by executing&#x20;

`git clone https://your-github-repo-here`

<figure><img src="../../../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

2\) Utilise a code editor such as [VSCode](https://code.visualstudio.com/) to prepare your IDE for development

3\) Inside the folder, fetch the latest widget-utils repository submodule:&#x20;

**NOTE:** If you would like to use your own widget utilities fork, you can also update the **.gitmodules** file and place the URL to your git repository there.

<figure><img src="../../../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

**Execute the following;**

`git submodule init && git submodule update`

4\) Install all packages by executing: `npm install`

5\) Execute `npm run start` to see our gallery of widgets in action.

6\) Access the following URL, substituting the widgetType with any widget you wish to preview.

{% embed url="https://localhost:4003/preview?widgetType=carousel" %}

Congratulations! You are now ready to start developing your first widget.&#x20;

Lets get started.

[creating-a-starter-project.md](creating-a-starter-project.md "mention")
