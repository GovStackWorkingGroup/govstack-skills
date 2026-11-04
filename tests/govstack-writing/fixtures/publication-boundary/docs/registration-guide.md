# Registration integration guide

This guide is about how a client submits a registration and reads the response. It is an
illustrative guide for the declared interface, and it is not evidence of a deployed service.

<a id="registration-request"></a>
## Submit a registration

`REQ-12 REQUIRED`

When the client submits a registration, it MUST send the source-issued `sourceReference`.
The service uses this value to relate the response to the source request.

From the repository root, the client can submit the illustrative request with this command:

```sh
registryctl registrations create --file examples/registration.json
```

The request body in `examples/registration.json` has this shape:

```json
{
  "sourceReference": "source-204",
  "description": "Keep this example description unchanged.",
  "summary": "Keep this example summary unchanged."
}
```

Read the [response example](#response-example) after sending the request.

<a id="response-example"></a>
## Response example

The response example repeats `sourceReference` so that the client can relate the response
to its source request. The example does not establish deployed behavior.

Return to the [submission requirement](#registration-request).
