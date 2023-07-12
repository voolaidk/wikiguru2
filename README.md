# Copilot Starter Kit

Using this starter kit, you can create your custom Copilot in less than 30 minutes for your own use case (sales, product, marketing, HR, etc) that build upon the master [RPM Copilot](https://rpm.sidekik.ai/) - **it's like AIner making babies** 👶

<img width="993" alt="Screenshot 2023-07-11 at 21 17 25" src="https://github.com/nftport/copilot-starter/assets/3767980/da115c3d-86ad-4aed-bb60-60ade38d663b">

# Video Tutorial

See [a 10 minute video tutorial](https://www.loom.com/share/da60c14a0d12429799f14a2b20e9fa1e?sid=0eeff877-da0a-4edc-8a6a-fe96bda7cf12) on how to create your own Copilot.

[![Watch the video](https://imgtr.ee/images/2023/07/12/91703daa686a33f1cdc0f776df56e4c1.png)]([[https://www.loom.com/share/da60c14a0d12429799f14a2b20e9fa1e?sid=0eeff877-da0a-4edc-8a6a-fe96bda7cf12]])

# Setup

### 1. Make a copilot config repository

On the top right of this page, click **Use this template** -> **Create a new repository**.

Fill in the required fields and click **Create repository from template**. We recommend keeping the repository private.

> ![](https://i.imgur.com/6qTJu7T.png)

Your newly created repository will hold all the configuration and data (PDFs, documents, etc) that defines how your copilot works.

### 2. Deploy the copilot

For your custom copilot to show up at rpm.sidekik.ai, you need to deploy it. To do so, go to [rpm.sidekik.ai](https://rpm.sidekik.ai), open the left sidebar, click "New copilot" on the top left, and fill in the fields. You can choose the name yourself; the Github link will be to the repository you just created.

> ![](https://i.imgur.com/oGzoDIb.png)

When you've submitted the form, you will get back a public key -- copy this. In order to deploy your copilot, we need read-access to your copilot repository. For this, you will add a deploy key to the repository you just created.

To do so, follow the provided link or navigate to your Copilot repository > Settings > Deploy keys. Press "Add deploy key".

> ![](https://i.imgur.com/zB5GJja.png)

Add a title, e.g. "Copilot deployment", and press "Add key". Write access is not needed.

> ![](https://i.imgur.com/psitGmT.png)

If you've successfully added a key, it should look like this:

> ![](https://i.imgur.com/y2z4ZGa.png)

Once done, we will automatically deploy your copilot. This may take 1-2 minutes.

### 3. Use your copilot

After successful deployment your copilot will show up at rpm.sidekik.ai. At the top left you can choose which copilot you chat with: the master RPM copilot or one of the custom copilots you created.

> ![](https://i.imgur.com/MTmJXQL.png)

After switching to the custom copilot you can start chatting!

### 4. Make it yours

Alright, you now have deployed a copilot, but you haven't really made it yours. The next section will show you how to customize your copilot.


# Customizing your copilot

There are three main ways you can customize your copilot: _Knowledge base_ and _Prompts_ that change the copilot's behaviour, and _Front-end configuration_ that changes how it is presented. To see these in action, you can check out several examples in the `examples/` directory -- to use one of the provided examples, just copy over the relevant files.

### Knowledge base

The [**knowledge base**](data/) is the set of documents that your copilot relies on when responding to you - you can think of it as the things you've taught it. The copilot has access to everything here when trying to answer your questions. To add things into the knowledge base, just drop files into `data/` directory.

The following file formats are supported:

* `pdf` files
* `csv`, `tsv` and `xls`/`xlsx` (Excel) files
* `txt` files

Many other file formats (e.g. Markdown, Powerpoint, ePub) also work but are not consistently tested -- they are parsed using the Python [unstructured](https://pypi.org/project/unstructured/) package.

### Prompts

In the [**prompt**](prompts/prompt_template.txt) you instruct your copilot about its goals, behaviour, style, etc. There aren't many hard rules to this; you can start from one of our provided [examples](https://github.com/nftport/copilot-starter/tree/master/examples) and iterate to improve the copilot's behaviour. Prompt engineering is a deep topic; for a more in-depth overview, see [OpenAI's list of prompting guides](https://github.com/openai/openai-cookbook#prompting-guides). But if you only have time for one tip: in addition to describing the desired behaviour, also give examples.

The prompt is not a static piece of text. It is a template that will be filled at runtime, after the user has messaged the copilot but before sending the request to the LLM. For this reason, you should always place three template variables into the prompt template file (ideally at the end), and they will be substituted as follows:

* `{context}`: the most relevant documents retrieved from your [knowledge base](#knowledge-base).
* `{history}`: the conversation history between the user and the copilot.
* `{question}`: the most recent input from the user -- the message they just sent.

### Front-end configuration

You can change some aspects of how the copilot is presented to the user, by changing the configuration in [`configuration.json`](/configuration.json). This screenshot shows visually what each setting in that file controls:

> ![](https://i.imgur.com/H4nTOJW.png)

Note that `description` is rendered as markdown, so you can add formatted text, images, and anything else that [markdown supports](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

### Giving access to other users

By default, only you (the creator of the custom copilot) can chat with your created copilot. You can easily give access to your team members so they can chat with it. Just add their email which they used to sign up to [master RPM Copilot](https://rpm.sidekik.ai/) to the `allowed_emails` list in [`configuration.json`](/configuration.json). The emails need to exactly match the email used to log into rpm.sidekik.ai. Adding the emails into this list will *not* send any notification.

## Integration

### REST API

You can use a REST API to integrate master RPM Copilot (AIner) anywhere you wish. It currently allows 10 concurrect requests. See this [gist script](https://gist.github.com/taivop/8c702809f60021c8280101301cc0f402) on how to call the API, or the [full API docs](https://api.sidekik.ai/docs#/). Currently, the REST API is unauthenticated (but a measure of privacy is provided by the fact that you can only access a chat's history if you know its uuid).

If you wish to integrate your own newly created custom Copilot, then please ask us for an API key.

## FAQ

**Who can access my custom copilot?**

By default, only the creator of the custom copilot has access to it.

**How do I share my custom copilot with a friend?**

1. Open the [`configuration.json`](/configuration.json) file.
1. Locate the `allowed_emails` list in the configuration.json file.
1. Add your friend's email to the allowed_emails list. Make sure the added email matches the email your friend uses to log into rpm.sidekik.ai.
1. Save the changes to the configuration.json file and commit.
1. Wait for the latest commit to be deployed.
1. Your friend should now see the custom copilot in the left sidebar, and be able to chat with it.

**Where is my data sent?**

* Everything you put into the Github repository is mirrored on our servers for ingestion and serving user messages.
* Upon every user message, a subset of the files in the `/data` directory gets sent to OpenAI, along with the prompt template, conversation history, and user message.

If you have concerns about adding some sensitive data, please talk to us before adding it to your custom copilot.