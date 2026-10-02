# AI Assistant

The AI Assistant answers questions about the workspace and current subsystem using packaged Help available to your role. It is not a general chat agent or a way to execute application operations.

## AI Assistant in use

![AI Assistant answering questions about copying a file and creating a book, with links to the relevant Help topics](images/ai-assistant-example.png)

The master Assistant searches authorized Help across subsystems. This user-supplied evaluation screenshot shows Files and Folders and Write a Book answers on the same page, with clearly distinguished **Read Help** links. Answers explain the workflow; they do not perform the operations. Generated guidance can be incorrect, so check the linked Help when a detail matters.

## Choose a response mode

- **Help-only** uses published Help without AI generation.
- **Local** uses the included AI model when allowed and available. No paid API request is made.
- **OpenAI** uses your own saved key and selected model when the Owner allows this provider and you explicitly choose paid use. Your provider can charge for requests.

Local is enabled and OpenAI disabled by default. There is no automatic paid fallback. If a mode cannot answer, do not assume another paid service was contacted. Each question is independent; the page retains up to six answers until you leave or clear it.

## Ask a question

The master page, **Ask about this software**, searches all published Help available to your role and enabled subsystems. Subsystem pages name their scope, such as **Ask about Document Library**, and search that subsystem's Help plus account guidance. Automatic selection is the default; choosing a topic supplies a hint rather than forcing an unrelated answer.

Type a non-empty question. **Ask** becomes available while typing, subject to request availability. Relevant excerpts are selected from the authorized collection rather than sending every Help page with every question. Answers include clearly styled **Read Help** links when appropriate. If no relevant guidance is found, the Assistant says so instead of displaying unrelated account instructions. Search relevance is currently English-oriented and can miss unusual wording; generated answers still require judgment.

Generated answers can be wrong. Check the application Help when a detail matters. An explanation is not evidence that an action has been performed. The Assistant cannot read your documents or mounted files, query application records, run commands or change data.

## Set up OpenAI

Open **AI Assistant → Assistant settings**. The settings panels start collapsed; select a panel heading to expand it.

1. **Owner enables the option:** expand **Owner: allowed help providers**, select **Allow OpenAI**, then **Save provider policy**. This makes the option available without selecting it or charging anyone.
2. **Each user saves their own key:** expand **My OpenAI key**, enter the key privately and choose **Save key**. The application does not display it again.
3. **Each user chooses paid use:** expand **My help provider**, select **OpenAI (paid)**, enter an available model ID compatible with the Responses API, select the paid-request authorization checkbox and choose **Save provider**.
4. Return to **AI Assistant**, leave **Use my selected provider for this question** checked and ask a Help question. Uncheck it to read Help without an AI request.

If OpenAI is disabled in the provider list, ask the Owner to allow it. Saving settings or a key does not itself make a paid request. Select **Local** or **Help-only** to stop using OpenAI for subsequent questions; there is no automatic paid fallback.

**Use a secured HTTPS connection before entering a real API key.** An unencrypted HTTP connection is not suitable for real credentials.

![Assistant settings showing personal provider selection, a blank write-only key field and the Owner provider policy](images/assistant-settings.jpg)

The screenshot uses a synthetic account with no saved key. OpenAI is disabled until the Owner allows it.

## Your OpenAI key

Each account supplies its own key. It is encrypted server-side and is not displayed again through application interfaces. Owner and Admin cannot retrieve or use another account's key through the application. Administrative password recovery clears the saved provider key.

Deleting a stored key does not revoke it at the provider. Revoke a compromised key there as well. A request already sent cannot be recalled. Never paste keys, passwords or private documents into a question, issue or screenshot.

OpenAI requests transmit your question and selected Help context to OpenAI. API charges are separate from WebAI. See [evaluation limitations](EVALUATION.md) for the current verification coverage.

## Planned data assistance

In future releases, AI Assistant is planned to answer questions about your application data through authorized database searches, within the current subsystem or across permitted subsystems from the master Assistant. This planned feature will require your own OpenAI connection and a suitable supported model; provider charges will apply. Availability and supported models will be specified with that release.

**This is not available in the current version.** Today's Assistant answers published Help only. Connecting OpenAI now does not enable database search or data changes, and does not grant additional access permissions.

## Security boundary

Write-only application access does not protect a key from someone controlling the host, Docker runtime or process memory, or possessing both the database and encryption keyring. Protect the surrounding environment and backups as described in [Security](SECURITY.md).

[Return to overview](README.md)
