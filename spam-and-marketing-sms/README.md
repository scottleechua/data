# spam and marketing SMS
*last updated: 13 Feb 2024*

Every text message I personally received since September 29, 2022, timestamped and tagged with its category.

## motivation

We usually consider spam as digital detritus: something that clogs the servers, something to *junk*, something to *purge*. We feel like this should be a solved problem by now, and yet the only texts we ever get are spam. 

But in the way one can learn a lot about someone by going through their trash — what they had for lunch, what they bought, what they buy into — perhaps one can also learn about a society by the spam it tries to erase.

Perhaps, perhaps not — we'll never know unless we study it. And we can't study it without first preserving it. Thus this dataset.

At the start of data collection, I wanted to eventually be able to chart my personal history with spam on a [timeline like this](https://spamofthetimes.com):

![Screenshot of a chart entitled 'spam of the times' showing the count of spam texts received per day betwen September 2022 and February 2024](https://raw.githubusercontent.com/scottleechua/spam-of-the-times/main/assets/header_spamofthetimes.png)

Consequently, this spam dataset has some unconventional features:

1. **Timestamps.** [Most spam datasets](https://www.kaggle.com/datasets/uciml/sms-spam-collection-dataset) don't have timestamps because they're constructed as training data for [spam detection models](https://archive.ics.uci.edu/dataset/228/sms+spam+collection), which just need the message text and the label. In contrast, a dataset with timestamps lets you explore the **seasonality** of spam: how do the messages change over time? is there more or less spam on holidays? how do events like[ mandatory SIM registration](https://www.philstar.com/headlines/2022/09/28/2212803/senate-approves-sim-registration-bill) affect spam volumes?
2. **More granular categories.** Instead of the usual binary label of spam vs. ham [(i.e., non-spam)](https://cwiki.apache.org/confluence/display/spamassassin/Ham), I sort texts into five categories — `spam`, `ads`, `gov`, `notifs`, or `OTP`. This better captures the reality that not all ham is equal: we treat OTPs and government PSAs very differently.
3. **Complete message history.** Every text message I've received since the start of data collection is a row in the dataset. While I do redact some messages and sender identities for privacy, I don't drop rows. This lets me ask: what fraction of messages in a given day or month were spam?

## categories
Each message is assigned one of the following categories:

1. `spam` — unsolicited messages from unknown senders.
2. `ads` — marketing messages from known businesses or services, such as GCash and Smart.
3. `gov` — public service announcements from government agencies, such as [NTC](https://ntc.gov.ph/) and [NDRRMC](https://ndrrmc.gov.ph/).
4. `notifs` — a catch-all category for legitimate and private messages, such as transaction confirmations, delivery updates, and a handful of personal conversations.
5. `OTP` — genuine one-time passwords.

## data dictionary

Dimensions: 3,083 rows x 5 columns.

Column | Definition
---|-------------
`date-received` | Datetime SMS was received in timezone `UTC+8`.
`date-read` | Datetime SMS was read in timezone `UTC+8`.
`sender` | A partially-masked phone number, unmasked alphanumeric [Sender ID](https://api.support.vonage.com/hc/en-us/articles/217571017-What-types-of-Sender-IDs-are-there), or one of three special values: <br> • `redacted_contact` if sender is a person in my personal contact book; <br>• `redacted_individual` if sender is a person not in my contacts and the message is solicited (e.g., updates from delivery riders); or <br> • `redacted_business` if sender is a business/service and **all** their messages are solicited.
`category` | Takes one of five possible values: `spam`, `ads`, `gov`, `notifs`, or `OTP`. See categories above.
`text` | • Full text for `spam`, `ads`, and `gov`. <br> • `<REDACTED>` for texts that are `notifs` or `OTP`.