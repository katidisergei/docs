---
title: Asynchronous WAV audio file recognition using the {{ speechkit-full-name }} API v3
description: Follow this guide to use asynchronous WAV audio file recognition in the API v3.
---

# Asynchronous WAV audio file recognition using the API v3

The example below illustrates how to use the {{ speechkit-name }} [API v3](../../stt-v3/api-ref/grpc/index.md) to perform asynchronous speech recognition on a WAV audio file. This example uses the following parameters:

* Audio stream format: WAV.
* [Speech recognition model](../models.md#tags): `general`.
* Other parameters are left at their defaults.

Authentication is performed under a service account using an [API key](../../../iam/concepts/authorization/api-key.md) or [IAM token](../../../iam/concepts/authorization/iam-token.md). For more information about authentication in the {{ speechkit-name }} API, see the [API reference](../../concepts/auth.md).

## Getting started {#before-you-begin}

{% include [transcribation-before-you-begin](../../../_includes/speechkit/transcribation-before-you-begin.md) %}

If you do not have a WAV audio file, you can use [this sample file](https://{{ s3-storage-host }}/doc-files/speech.wav).

## Perform speech recognition via the API v3 {#recognize-using-api-v3}

{% list tabs group=programming_language %}

- cURL {#curl}

  {% include [transcribation-before-you-begin](../../../_includes/speechkit/async-recognition-v3.md) %}

- Python 3 {#python}

  1. Clone the [{{ yandex-cloud }} API](https://github.com/yandex-cloud/cloudapi) repository:

      ```bash
      git clone https://github.com/yandex-cloud/cloudapi
      ```

  1. Use the [pip package](https://pip.pypa.io/en/stable/) manager to install the `grpcio-tools` package:

      ```bash
      pip install grpcio-tools
      ```

  1. Go to the folder hosting the cloned {{ yandex-cloud }} API repository, create a folder named `output`, and generate the client interface code there:

      ```bash
      cd <path_to_cloudapi_folder>
      mkdir output
      python3 -m grpc_tools.protoc -I . -I third_party/googleapis \
        --python_out=output \
        --grpc_python_out=output \
        google/api/http.proto \
        google/api/annotations.proto \
        yandex/cloud/api/operation.proto \
        google/rpc/status.proto \
        yandex/cloud/operation/operation.proto \
        yandex/cloud/validation.proto \
        yandex/cloud/ai/stt/v3/stt_service.proto \
        yandex/cloud/ai/stt/v3/stt.proto
      ```

      The `stt_pb2.py`, `stt_pb2_grpc.py`, `stt_service_pb2.py`, and `stt_service_pb2_grpc.py` client interface files, as well as dependency files, will be created in the `output` folder.

  1. Create a file, e.g., `test.py`, in the `output` folder root and add the following API request code to it:

      ```python
      import grpc
      from yandex.cloud.ai.stt.v3 import stt_pb2, stt_service_pb2_grpc

      request = stt_pb2.RecognizeFileRequest(
        uri='https://storage.yandexcloud.net/<bucket_name>/<path_to_WAV_file_in_bucket>',
        recognition_model=stt_pb2.RecognitionModelOptions(
          model='general',
          audio_format=stt_pb2.AudioFormatOptions(
            container_audio=stt_pb2.ContainerAudio(
              container_audio_type=stt_pb2.ContainerAudio.WAV
            )
          )
        )
      )

      cred = grpc.ssl_channel_credentials()
      chan = grpc.secure_channel('stt.{{ api-host }}:443', cred)
      stub = stt_service_pb2_grpc.AsyncRecognizerStub(chan)

      # Choose one of the authentication methods:

      # Authentication with an IAM token
      response = stub.RecognizeFile(request, metadata=[('authorization', 'Bearer <IAM_token>')])

      # Authentication with an API key
      # response = stub.RecognizeFile(request, metadata=[('authorization', 'Api-Key <API_key>')])

      print(response)
      ```

  1. Run the following query:

      ```bash
      python3 test.py
      ```

      Result:

      ```bash
      id: "f8dem628l2mq********"
      description: "STT v3 async recognition"
      created_at {
        seconds: 1721032219
      }
      created_by: "ajehumcuv38h********"
      modified_at {
        seconds: 1721032219
      }
      ```

      Save the recognition operation `id` you get in the response.

  1. Create a file, e.g., `results.py`, in the `output` folder root and add the following code to it to get the operation result:

      ```python
      import grpc
      from yandex.cloud.ai.stt.v3 import stt_pb2, stt_service_pb2_grpc, stt_service_pb2

      request = stt_service_pb2.GetRecognitionRequest(
          operation_id="<operation_ID>"
      )

      cred = grpc.ssl_channel_credentials()
      chan = grpc.secure_channel('stt.{{ api-host }}:443', cred)
      stub = stt_service_pb2_grpc.AsyncRecognizerStub(chan)

      # Authentication with an IAM token
      response = stub.GetRecognition(request, metadata=[('authorization', 'Bearer <IAM_token>')])

      # Authentication with an API key
      # response = stub.GetRecognition(request, metadata=[('authorization', 'Api-Key <API_key>')])

      print(list(response))
      ```

  1. Run the following query:

      ```bash
      python3 results.py
      ```

      {% cut "Result example" %}

      ```bash
      [session_uuid {
        uuid: "df49eaa2-25a55218-ae967fa1-********"
        user_request_id: "f8dkup42nmhk********"
      }
      audio_cursors {
        received_data_ms: 6600
        partial_time_ms: 6600
        final_time_ms: 6600
      }
      response_wall_time_ms: 204
      final {
        alternatives {
          words {
            text: "I'm"
            start_time_ms: 380
            end_time_ms: 420
          }
          words {
            "text": "Yandex"
            start_time_ms: 539
            end_time_ms: 919
          }
          words {
            "text": "SpeechKit"
            start_time_ms: 960
            end_time_ms: 1719
          }
          words {
            text: "I"
            start_time_ms: 2159
            end_time_ms: 2200
          }
          words {
            "text": "can"
            start_time_ms: 2260
            end_time_ms: 2440
          }
          words {
            text: "turn"
            start_time_ms: 2520
            end_time_ms: 3000
          }
          words {
            "text": "any"
            start_time_ms: 3060
            end_time_ms: 3320
          }
          words {
            "text": "text"
            start_time_ms: 3419
            end_time_ms: 3740
          }
          words {
            "text": "into"
            start_time_ms: 3780
            end_time_ms: 3800
          }
          words {
            "text": "speech"
            start_time_ms: 3860
            end_time_ms: 4279
          }
          words {
            "text": "now"
            start_time_ms: 4680
            end_time_ms: 5240
          }
          words {
            "text": "you"
            start_time_ms: 5339
            end_time_ms: 5380
          }
          words {
            "text": "can"
            start_time_ms: 5460
            end_time_ms: 5766
          }
          words {
            text: "too"
            start_time_ms: 5920
            end_time_ms: 6393
          }
          text: "I'm Yandex SpeechKit I can turn any text into speech now you can too"
          end_time_ms: 6600
        }
        channel_tag: "0"
      }
      channel_tag: "0"
      , session_uuid {
        uuid: "df49eaa2-25a55218-ae967fa1-********"
        user_request_id: "f8dkup42nmhk********"
      }
      audio_cursors {
        received_data_ms: 6600
        partial_time_ms: 6600
        final_time_ms: 6600
      }
      response_wall_time_ms: 204
      final_refinement {
        normalized_text {
          alternatives {
            words {
              text: "I'm"
              start_time_ms: 380
              end_time_ms: 420
            }
            words {
              "text": "Yandex"
              start_time_ms: 539
              end_time_ms: 919
            }
            words {
              "text": "SpeechKit"
              start_time_ms: 960
              end_time_ms: 1719
            }
            words {
              text: "I'm"
              start_time_ms: 2159
              end_time_ms: 2200
            }
            words {
              "text": "can"
              start_time_ms: 2260
              end_time_ms: 2440
            }
            words {
              text: "turn"
              start_time_ms: 2520
              end_time_ms: 3000
            }
            words {
              "text": "any"
              start_time_ms: 3060
              end_time_ms: 3320
            }
            words {
              "text": "text"
              start_time_ms: 3419
              end_time_ms: 3740
            }
            words {
              "text": "into"
              start_time_ms: 3780
              end_time_ms: 3800
            }
            words {
              "text": "speech"
              start_time_ms: 3860
              end_time_ms: 4279
            }
            words {
              "text": "now"
              start_time_ms: 4680
              end_time_ms: 5240
            }
            words {
              "text": "you"
              start_time_ms: 5339
              end_time_ms: 5380
            }
            words {
              "text": "can"
              start_time_ms: 5460
              end_time_ms: 5766
            }
            words {
              text: "too"
              start_time_ms: 5920
              end_time_ms: 6393
            }
            text: "I'm Yandex SpeechKit I can turn any text into speech now you can too"
            end_time_ms: 6600
          }
          channel_tag: "0"
        }
      }
      channel_tag: "0"
      , session_uuid {
        uuid: "df49eaa2-25a55218-ae967fa1-********"
        user_request_id: "f8dkup42nmhk********"
      }
      audio_cursors {
        received_data_ms: 6600
        partial_time_ms: 6600
        final_time_ms: 6600
        eou_time_ms: 6600
      }
      response_wall_time_ms: 204
      eou_update {
        time_ms: 6600
      }
      channel_tag: "0"
      ]
      ```

      {% endcut %}

{% endlist %}