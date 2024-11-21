---
editable: false
sourcePath: en/_api-ref/ai/assistants/v1/runs/api-ref/Run/listen.md
---

# Runs, REST: Run.Listen {#Listen}

Listen to events from a specific run.
If the run was created with `stream = false`, Listen will only respond with the final status of the run
and will not stream partial messages or intermediate events.

## HTTP request

```
GET https://rest-assistant.{{ api-host }}/assistants/v1/runs/listen
```

## Query parameters {#yandex.cloud.ai.assistants.v1.runs.ListenRunRequest}

Request message for listing to a run events.

#|
||Field | Description ||
|| runId | **string**

Required field. ID of the run to listen to. ||
|| eventsStartIdx | **string** (int64)

Starting index for events. If provided, listening will start from this event. ||
|#

## Response {#yandex.cloud.ai.assistants.v1.runs.StreamEvent}

**HTTP Code: 200 - OK**

```json
{
  "eventType": "string",
  "streamCursor": {
    "currentEventIdx": "string",
    "numUserEventsReceived": "string"
  },
  // Includes only one of the fields `error`, `partialMessage`, `completedMessage`
  "error": {
    "code": "string",
    "message": "string"
  },
  "partialMessage": {
    "content": [
      {
        // Includes only one of the fields `text`
        "text": {
          "content": "string"
        }
        // end of the list of possible fields
      }
    ]
  },
  "completedMessage": {
    "id": "string",
    "threadId": "string",
    "createdBy": "string",
    "createdAt": "string",
    "author": {
      "id": "string",
      "role": "string"
    },
    "labels": "string",
    "content": {
      "content": [
        {
          // Includes only one of the fields `text`
          "text": {
            "content": "string"
          }
          // end of the list of possible fields
        }
      ]
    },
    "status": "string"
  }
  // end of the list of possible fields
}
```

Represents an event in the stream of a run.

#|
||Field | Description ||
|| eventType | **enum** (EventType)

The type of event.

- `EVENT_TYPE_UNSPECIFIED`: Unspecified event type.
- `PARTIAL_MESSAGE`: Partial message is available.
- `ERROR`: Run has failed due to an error.
- `DONE`: The run has completed. ||
|| streamCursor | **[StreamCursor](#yandex.cloud.ai.assistants.v1.runs.StreamCursor)**

The current position in the stream. ||
|| error | **[Error](#yandex.cloud.ai.common.Error)**

Error information if the run has failed.

Includes only one of the fields `error`, `partialMessage`, `completedMessage`.

Event data, which can be one of several types. ||
|| partialMessage | **[MessageContent](#yandex.cloud.ai.assistants.v1.threads.MessageContent)**

Partially generated message.

Includes only one of the fields `error`, `partialMessage`, `completedMessage`.

Event data, which can be one of several types. ||
|| completedMessage | **[Message](#yandex.cloud.ai.assistants.v1.threads.Message)**

Final message generated by an assistant if a run has completed successfully.

Includes only one of the fields `error`, `partialMessage`, `completedMessage`.

Event data, which can be one of several types. ||
|#

## StreamCursor {#yandex.cloud.ai.assistants.v1.runs.StreamCursor}

Represents the cursor position in a stream of events.

#|
||Field | Description ||
|| currentEventIdx | **string** (int64)

Index of the current event in the stream. ||
|| numUserEventsReceived | **string** (int64)

The number of user events received so far. ||
|#

## Error {#yandex.cloud.ai.common.Error}

#|
||Field | Description ||
|| code | **string** (int64) ||
|| message | **string** ||
|#

## MessageContent {#yandex.cloud.ai.assistants.v1.threads.MessageContent}

Represents the content of a message, which can consist of multiple parts.

#|
||Field | Description ||
|| content[] | **[ContentPart](#yandex.cloud.ai.assistants.v1.threads.ContentPart)**

A list of content parts that make up the message. ||
|#

## ContentPart {#yandex.cloud.ai.assistants.v1.threads.ContentPart}

ContentPart represents an individual part of the message content, which can be of various types.

#|
||Field | Description ||
|| text | **[Text](#yandex.cloud.ai.assistants.v1.threads.Text)**

Text content of the message part.

Includes only one of the fields `text`.

Specifies the type of content that the part contains. ||
|#

## Text {#yandex.cloud.ai.assistants.v1.threads.Text}

Text represents a textual content part of a message.

#|
||Field | Description ||
|| content | **string**

Text content of the message. ||
|#

## Message {#yandex.cloud.ai.assistants.v1.threads.Message}

#|
||Field | Description ||
|| id | **string**

Unique identifier of the message. ||
|| threadId | **string**

ID of the thread that this message belongs to. ||
|| createdBy | **string**

Identifier of the subject who created this message. ||
|| createdAt | **string** (date-time)

Timestamp representing when the message was created.

String in [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) text format. The range of possible values is from
`0001-01-01T00:00:00Z` to `9999-12-31T23:59:59.999999999Z`, i.e. from 0 to 9 digits for fractions of a second.

To work with values in this field, use the APIs described in the
[Protocol Buffers reference](https://developers.google.com/protocol-buffers/docs/reference/overview).
In some languages, built-in datetime utilities do not support nanosecond precision (9 digits). ||
|| author | **[Author](#yandex.cloud.ai.assistants.v1.threads.Author)**

Author of the message, containing details about the message's creator. ||
|| labels | **string**

Set of key-value pairs that can be used to organize and categorize the message. ||
|| content | **[MessageContent](#yandex.cloud.ai.assistants.v1.threads.MessageContent)**

Content of the message. ||
|| status | **enum** (MessageStatus)

Status of the message.

- `MESSAGE_STATUS_UNSPECIFIED`: Unspecified message status.
- `COMPLETED`: Message was successfully created by a user or generated by an assistant.
- `TRUNCATED`: Message generation was truncated due to reaching the maximum allowed number of tokens.
- `FILTERED_CONTENT`: Message generation was stopped because potentially sensitive content was detected either in the prompt or in the generated response. ||
|#

## Author {#yandex.cloud.ai.assistants.v1.threads.Author}

Author of the message, containing details about the message's creator.

#|
||Field | Description ||
|| id | **string**

Unique identifier of the author. This could be either the user's ID or the assistant's ID, depending on the role. ||
|| role | **string**

Role of the author, indicating whether the message was created by a "user" or an "assistant". ||
|#