---
title: DICOM Networking
type: docs
weight: 100
url: /net/developer-guide/dicom-networking/
---

# DICOM **Networking**

`Aspose.Medical.Dicom.Network` provides DICOM association requestors and acceptors for DIMSE operations. The client opens outbound associations and sends queued requests. The server listens for inbound associations and dispatches received DIMSE requests to registered handlers.

The public API is grouped around these entry points:

| API area | Public types |
| --- | --- |
| Client | `DicomNetworkClient`, `DicomNetworkClientOptions`, `DicomNetworkClientBuilder` |
| Server | `DicomNetworkServer`, `DicomNetworkServerOptions`, `DicomNetworkServerBuilder` |
| Transport | `DicomNetworkConnectionOptions`, `DataTransferChannelOptions`, `ITlsAuthenticator`, `TlsInitiatorAuthenticator`, `TlsAcceptorAuthenticator` |
| Association negotiation | `AssociationNegotiationOptions`, `AssociationTimeoutOptions`, `PresentationContext`, `PresentationContexts`, `AsyncOperationsWindow`, `ExtendedNegotiation`, `ExtendedNegotiations`, `ServiceClassRole`, `UserIdentityNegotiationDefinition` |
| DIMSE message model | `IDicomMessage`, `IDicomRequest`, `IDicomRequest<TResponse>`, `IDicomStreamRequest<TResponse>`, `IDicomResponse`, concrete C-service and N-service messages listed below |
| Handler pipeline | Operation-specific DIMSE request handlers and exchange handlers, plus endpoint-level generic request, stream, behavior, factory, and notification registrations |
| Network notifications | `AssociationAcceptedNotification`, `AssociationRejectedNotification`, `AssociationReleasedNotification`, `AssociationReleaseRequestedNotification`, `AssociationEstablishmentTimedOutNotification`, `AssociationReleaseResponseTimedOutNotification`, `AssociationAbortReceivedNotification`, `AssociationAbortSentNotification`, `RequestTimedOutNotification`, `UnaryExchangeNotification<TRequest,TResponse>`, `StreamExchangeNotification<TRequest,TResponse>` |
| Established association state | `EffectiveAssociation`, `EffectivePresentationContext` |
| Association control | `AssociationRequested`, `AssociationRequestedResult`, `AssociationAcceptedResult`, `AssociationRejectedResult`, `AssociationRequest`, `AssociationAccept`, presentation-context, role-selection, extended-negotiation, user-identity, reject, abort, and release control types listed below |
| Errors | `DicomNetworkException`, `AssociationRejectedException`, `AssociationAbortException`, `AssociationReleasedException`, `AsyncOperationsWindowExceededException`, `InvalidAssociationTransitionException`, `PduReadException` |

## Settings

Client options:

| Setting | Default | Meaning |
| --- | --- | --- |
| `Connection` | required | Remote endpoint and transport options used to open associations |
| `AssociationNegotiation` | required | AE titles, presentation contexts, roles, extended negotiation, async limits, and association timers proposed by the client |
| `Runtime` | `Scheduler = TaskScheduler.Default`, `Pool = ArrayPool<byte>.Shared` | Scheduler and byte-pool services shared by client sessions |
| `OperationResponseTimeout` | 1 minute | Time to wait for the next response to a DIMSE request sent by this endpoint |
| `MaxRequestsPerAssociation` | unlimited | Maximum queued requests sent on one association; `<= 0` is normalized to unlimited |
| `UserIdentity` | `null` | Optional User Identity Negotiation item sent in A-ASSOCIATE-RQ |

Server options:

| Setting | Default | Meaning |
| --- | --- | --- |
| `Connection` | required | Listen endpoint and transport options used by accepted connections |
| `AssociationNegotiation` | required | Server-side AE titles, supported presentation contexts, roles, extended negotiation, async limits, and association timers |
| `Runtime` | `Scheduler = TaskScheduler.Default`, `Pool = ArrayPool<byte>.Shared` | Scheduler and byte-pool services shared by accepted sessions |
| `OperationResponseTimeout` | 1 minute | Time to wait for the next response to a DIMSE request sent by this endpoint, including sub-operations |
| `NoDelay` | `true` | Disables Nagle's algorithm on accepted TCP sockets |
| `Backlog` | `512` | Pending TCP connection queue length |
| `UserIdentityResponse` | `null` | Positive user-identity response returned by the acceptor when requested |

Runtime options:

| Setting | Default | Meaning |
| --- | --- | --- |
| `Scheduler` | `TaskScheduler.Default` | Scheduler used for asynchronous networking callbacks |
| `Pool` | `ArrayPool<byte>.Shared` | Byte array pool used by network buffers |

Connection options:

| Setting | Default | Meaning |
| --- | --- | --- |
| `TargetHost` | required | Client remote endpoint or server listen endpoint |
| `TlsAuthenticator` | `null` | TLS authenticator; `null` means plain TCP |
| `NoDelay` | `false` | Disables Nagle's algorithm on the socket when enabled |
| `ConnectTimeout` | 10 seconds | TCP connect budget before transport exists |
| `SendTimeout` | infinite | Budget for one asynchronous transport write |
| `ReceiveTimeout` | infinite | Budget for one asynchronous transport read |
| `CloseTimeout` | 10 seconds | Budget for transport worker loops to finish during shutdown |
| `InboundChannelOptions` | `DataTransferChannelOptions.Default` | Pipe settings for received bytes |
| `OutboundChannelOptions` | `DataTransferChannelOptions.Default` | Pipe settings for bytes to send |

Data transfer channel options:

| Setting | Default | Meaning |
| --- | --- | --- |
| `BufferSize` | system default | Pipe buffer size; non-positive values become `null` |
| `MinimumSegmentSize` | system default | Minimum pipe segment size; non-positive values become `null` |
| `PauseWriterThreshold` | system default | Buffered byte count where writing pauses; negative values become `null` |
| `ResumeWriterThreshold` | system default | Buffered byte count where writing resumes; negative values become `null` |
| `UseSynchronizationContext` | `false` | Whether pipe continuations capture the current synchronization context |

Built-in TLS authenticators:

`DicomNetworkConnectionOptions.TlsAuthenticator` accepts any `ITlsAuthenticator`. `null` uses plain TCP. The library provides two default implementations:

| Type | Use | Contract |
| --- | --- | --- |
| `TlsInitiatorAuthenticator` | Side that opens the transport connection | Authenticates the `SslStream` as TLS client |
| `TlsAcceptorAuthenticator` | Side that accepts the transport connection | Authenticates the `SslStream` as TLS server |

Common built-in authenticator properties:

| Property | Default | Meaning |
| --- | --- | --- |
| `EnabledProtocols` | TLS 1.2 and TLS 1.3 | Optional protocol set |
| `Timeout` | 1 minute | TLS authentication budget |
| `ValidationCallback` | `null` | Custom remote-certificate validation callback |
| `CipherSuitesPolicy` | `null` | Optional cipher-suite policy |
| `CertificateChainPolicy` | `null` | Optional certificate-chain validation policy |
| `CertificateRevocationCheckMode` | `Online` | Revocation check mode during authentication |

`TlsInitiatorAuthenticator` properties:

| Property | Default | Meaning |
| --- | --- | --- |
| `TargetHost` | required | TLS server name used for SNI and remote certificate validation |
| `ClientCertificates` | `null` | Optional client certificates presented during authentication |

`TlsAcceptorAuthenticator` properties:

| Property | Default | Meaning |
| --- | --- | --- |
| `TargetHost` | required | TLS host name represented by the served certificate |
| `CertificateSelectionCallback` | `null` | Certificate selector; used before certificate context and certificate properties |
| `ServerCertificateContext` | `null` | Certificate context presented when no selection callback is set |
| `ServerCertificate` | `null` | Certificate presented when no selection callback or context is set |
| `ClientCertificateRequired` | `false` | Requires the initiator to present a client certificate |

For the built-in acceptor, certificate source precedence is `CertificateSelectionCallback`, then `ServerCertificateContext`, then `ServerCertificate`. If none is provided, the built-in selector throws during TLS authentication.

Association negotiation settings:

| Setting | Default | Meaning |
| --- | --- | --- |
| `Called` | required | Called AE title; spaces are trimmed |
| `Calling` | required | Calling AE title; spaces are trimmed |
| `MaxPduLength` | `262144` | Maximum PDU body length advertised as Maximum Length Received |
| `Timeouts` | `new()` | Association open, release, and close timer budgets |
| `FallbackEncoding` | ASCII | Text encoding used when a DIMSE dataset has no specific character set |
| `AsyncOperationsLimits` | synchronous `(1, 1)` | Asynchronous operations window proposed or supported |
| `PresentationContexts` | empty | SOP Classes, transfer syntaxes, and local service-class roles proposed or supported |
| `ExtendedNegotiations` | empty | SOP Class extended and common extended negotiation items |

Association timeout settings:

| Setting | Default | Meaning |
| --- | --- | --- |
| `OpenTimeout` | 5 seconds | Wait for A-ASSOCIATE-AC or A-ASSOCIATE-RJ after the transport opens |
| `ReleaseTimeout` | 10 seconds | Wait for A-RELEASE-RP after local A-RELEASE-RQ |
| `CloseTimeout` | 10 seconds | Wait for transport close after local reject, release response, or abort |

Presentation context settings:

| Setting | Default | Meaning |
| --- | --- | --- |
| `AbstractSyntax` | required | SOP Class or information model UID |
| `TransferSyntaxes` | required | Transfer syntaxes to propose or support; must contain at least one item |
| `Role` | required, usually `null` | Local AE role. `null` uses DICOM defaults: requestor is SCU, acceptor is SCP |

Role configuration is local to the endpoint that owns the options. DICOM role-selection items on the wire are requestor-relative, so server-side capabilities are mirrored during negotiation:

| Stage | Mapping |
| --- | --- |
| Client configuration | Non-null `Role` is sent in A-ASSOCIATE-RQ unchanged because role-selection items describe the requestor |
| Server configuration | Local `Provider` accepts requestor `User`; local `User` accepts requestor `Provider`; both local roles accept both requestor roles |
| Server default | `Role = null` means default roles and accepts requestor `User` |
| Server acceptance | Accepted requestor role is `requestor proposal & mirrored server capability` |
| Client effective role | `EffectivePresentationContext.LocalRole` is `client proposal & accepted requestor role`; omitted role-selection result defaults to local `User` |
| Server effective role | Accepted requestor `User` becomes local `Provider`; accepted requestor `Provider` becomes local `User`; omitted role-selection result defaults to local `Provider` |

After negotiation, `EffectivePresentationContext.LocalRole` is local to the endpoint that owns it. Sending a DIMSE request requires `ServiceClassRole.User`; handling an inbound DIMSE request requires `ServiceClassRole.Provider`. Use `ServiceClassRole.User | ServiceClassRole.Provider` when one AE must both send and receive the same SOP Class.

Async operations window:

| Setting | Meaning |
| --- | --- |
| `MaxOperationsInvoked` | Maximum concurrent operations the requestor may invoke; `0` means unlimited |
| `MaxOperationsPerformed` | Maximum concurrent operations the acceptor may perform; `0` means unlimited |

Constructor values are clamped to the DICOM wire range `0..65535`. `(1, 1)` is synchronous operation mode.

Extended negotiation settings:

| Type | Setting | Meaning |
| --- | --- | --- |
| `ExtendedNegotiation` | `SopClass` | SOP Class UID the item applies to |
| `ExtendedNegotiation` | `ApplicationInfo` | SOP Class Extended Negotiation application information |
| `ExtendedNegotiation` | `ServiceClass` | Service Class UID for Common Extended Negotiation |
| `ExtendedNegotiation` | `RelatedGeneralSopClasses` | Related general SOP Classes for Common Extended Negotiation |
| `ExtendedNegotiations` | `Count` | Number of configured items; `With(...)` adds or replaces by SOP Class |
| `ServiceClassApplicationInfo` | raw fields | Generic application information when no typed helper is available |
| `CFindApplicationInfo` | `RelationalQueries`, `DateTimeMatching`, `FuzzySemanticMatching`, `TimezoneQueryAdjustment`, `EnhancedMultiFrameImageConversion`, `EmptyValueMatching`, `MultipleValueMatching` | C-FIND extended negotiation flags |
| `CGetApplicationInfo` | `RelationalRetrieval`, `EnhancedMultiFrameImageConversion` | C-GET extended negotiation flags |
| `CMoveApplicationInfo` | `RelationalRetrieval`, `EnhancedMultiFrameImageConversion` | C-MOVE extended negotiation flags |
| `CStoreApplicationInfo` | `SupportLevel`, `DigitalSignatureSupportLevel`, `Coercion` | Storage Service Class extended negotiation values |

User identity settings:

| Setting | Meaning |
| --- | --- |
| `UserIdentityType` | `Username`, `UsernameAndPasscode`, `Kerberos`, `Saml`, or `Jwt` |
| `PositiveResponseRequested` | Whether the acceptor should return a positive user-identity response |
| `PrimaryField` | Username, Kerberos ticket, SAML assertion, JWT, or other primary identity payload |
| `SecondaryField` | Passcode when `UserIdentityType` is `UsernameAndPasscode`; otherwise usually `null` |

## DIMSE Message Types

All concrete DIMSE messages implement `IDicomMessage`. Requests also implement `IDicomRequest`; unary requests use `IDicomRequest<TResponse>`, and multi-response requests use `IDicomStreamRequest<TResponse>`. Responses implement `IDicomResponse`.

C-service messages:

| DIMSE operation | Request type | Response type | Flow | Server handler | Client exchange handler |
| --- | --- | --- | --- | --- | --- |
| C-CANCEL | `CCancelRequest` | `NoneDicomNetworkResponse` | control request | `ICCancelRequestHandler` | `ICCancelExchangeHandler` |
| C-ECHO | `CEchoRequest` | `CEchoResponse` | unary | `ICEchoRequestHandler` | `ICEchoExchangeHandler` |
| C-STORE | `CStoreRequest` | `CStoreResponse` | unary | `ICStoreRequestHandler` | `ICStoreExchangeHandler` |
| C-FIND | `CFindRequest` | `CFindResponse` | stream | `ICFindRequestHandler` | `ICFindExchangeHandler` |
| C-GET | `CGetRequest` | `CGetResponse` | stream | `ICGetRequestHandler` | `ICGetExchangeHandler` |
| C-MOVE | `CMoveRequest` | `CMoveResponse` | stream | `ICMoveRequestHandler` | `ICMoveExchangeHandler` |

N-service messages:

| DIMSE operation | Request type | Response type | Flow | Server handler | Client exchange handler |
| --- | --- | --- | --- | --- | --- |
| N-EVENT-REPORT | `NEventReportRequest` | `NEventReportResponse` | unary | `INEventReportRequestHandler` | `INEventReportExchangeHandler` |
| N-GET | `NGetRequest` | `NGetResponse` | unary | `INGetRequestHandler` | `INGetExchangeHandler` |
| N-SET | `NSetRequest` | `NSetResponse` | unary | `INSetRequestHandler` | `INSetExchangeHandler` |
| N-ACTION | `NActionRequest` | `NActionResponse` | unary | `INActionRequestHandler` | `INActionExchangeHandler` |
| N-CREATE | `NCreateRequest` | `NCreateResponse` | unary | `INCreateRequestHandler` | `INCreateExchangeHandler` |
| N-DELETE | `NDeleteRequest` | `NDeleteResponse` | unary | `INDeleteRequestHandler` | `INDeleteExchangeHandler` |

`C-FIND`, `C-GET`, and `C-MOVE` can return pending responses before the terminal response. `C-CANCEL` has no DIMSE response on the wire; `NoneDicomNetworkResponse` is the typed completion value used by the handler pipeline.

Supporting message types:

| Type | Role |
| --- | --- |
| `IDicomMessage` | Common command dataset plus optional data dataset |
| `IDicomRequest` | Request message ID, SOP Class UID, association context, pending-response support, and cancellation support |
| `IDicomRequest<TResponse>` | Unary DIMSE request contract |
| `IDicomStreamRequest<TResponse>` | Multi-response DIMSE request contract |
| `IDicomResponse` | Response message ID link and terminal-response marker |
| `IDicomRequestContext` | Association-bound context available to request handlers |
| `CServiceRequest` | Base class for typed C-service requests except `CCancelRequest` |
| `NLifecycleRequest<TResponse>` | Base class for N-CREATE and N-EVENT-REPORT requests |
| `NTargetedRequest<TResponse>` | Base class for N-GET, N-SET, N-ACTION, and N-DELETE requests |
| `NoneDicomNetworkResponse` | Singleton response value for request routes that do not produce a DIMSE response |
| `RequestTimedOutNotification` | Notification published when a locally invoked DIMSE request reaches `OperationResponseTimeout` |

Related option and enum types:

| Type | Use |
| --- | --- |
| `DimsePriority` | Requested DIMSE operation priority |
| `DimseQueryRetrieveLevel` | Query/retrieve level value for C-FIND, C-GET, and C-MOVE identifiers |
| `CFindOptions`, `CGetOptions`, `CMoveOptions` | Query/retrieve SOP Class extended-negotiation flags |
| `DigitalSignatureSupportLevels`, `ElementCoercion`, `SupportLevels` | Storage SOP Class extended-negotiation values |
| `DicomNetworkServices` | DIMSE command-field values |

DIMSE request fields:

| Request | Settings and constructor data |
| --- | --- |
| `CEchoRequest` | No payload or extra settings; targets `Uid.Verification` |
| `CCancelRequest` | `messageIdBeingRespondedTo` identifies the active `C-FIND`, `C-GET`, or `C-MOVE` to cancel |
| `CStoreRequest` | `dataset`, `transferSyntax`, `Priority`, `AdditionalTransferSyntaxes`, `OmitImplicitVrTransferSyntaxInAssociationRequest` |
| `CFindRequest` | `Identifier`, `Priority`, `Level`, `PreferredPresentationContexts`; create with patient, study, series, image, or worklist helpers |
| `CGetRequest` | `Dataset`, `Priority`, `Level`, `PreferredPresentationContexts`; create with study, series, or image helpers |
| `CMoveRequest` | `Identifier`, `MoveDestination`, `Priority`, `Level`, `PreferredPresentationContexts`; create with study, series, or image helpers |
| `NEventReportRequest` | affected SOP Class/Instance, `EventTypeId`, optional `EventInformation`, `PreferredPresentationContexts` |
| `NGetRequest` | requested SOP Class/Instance and `AttributeIdentifierList` |
| `NSetRequest` | requested SOP Class/Instance, `ModificationList`, `PreferredPresentationContexts` |
| `NActionRequest` | requested SOP Class/Instance, `ActionTypeId`, optional `ActionInformation`, `PreferredPresentationContexts` |
| `NCreateRequest` | affected SOP Class/Instance, optional `Attributes`, `PreferredPresentationContexts` |
| `NDeleteRequest` | requested SOP Class/Instance |

`PreferredPresentationContexts` adds request-specific presentation context proposals before the automatic default proposal for that request's SOP Class. `CStoreRequest` instead derives proposals from the dataset SOP Class and transfer syntax; `AdditionalTransferSyntaxes` adds extra candidates.

DIMSE response fields:

| Response | Main fields |
| --- | --- |
| `CEchoResponse` | `Status`, `AffectedSopClassUid` |
| `CStoreResponse` | `Status`, affected SOP Class/Instance, `ErrorComment`, `ErrorId` |
| `CFindResponse` | `Status`, `Identifier`, affected SOP Class, sub-operation counters |
| `CGetResponse` | `Status`, affected SOP Class, `Remaining`, `Completed`, `Failures`, `Warnings`, `ErrorComment`, `ErrorId` |
| `CMoveResponse` | `Status`, affected SOP Class, `Remaining`, `Completed`, `Failures`, `Warnings` |
| `NEventReportResponse` | `Status`, affected SOP Class/Instance, `EventTypeId`, `EventReply` |
| `NGetResponse`, `NSetResponse`, `NCreateResponse` | `Status`, affected SOP Class/Instance, optional `Attributes` |
| `NActionResponse` | `Status`, affected SOP Class/Instance, `ActionTypeId`, optional `ActionReply` |
| `NDeleteResponse` | `Status`, affected SOP Class/Instance |

## Configure a Connection

`DicomNetworkConnectionOptions` describes the transport endpoint and optional TLS. `TargetHost` is required. Client options use it as the remote endpoint; server options use it as the listen endpoint.

```csharp
using System.Net;
using Aspose.Medical.Dicom.Network;
using Aspose.Medical.Dicom.Network.Connection;

DicomNetworkConnectionOptions connection = new()
{
    TargetHost = new IPEndPoint(IPAddress.Parse("192.0.2.10"), 104),
    NoDelay = true,
    ConnectTimeout = TimeSpan.FromSeconds(10),
    SendTimeout = TimeSpan.FromSeconds(15),
    ReceiveTimeout = TimeSpan.FromSeconds(15),
    CloseTimeout = TimeSpan.FromSeconds(10),
    InboundChannelOptions = DataTransferChannelOptions.Default,
    OutboundChannelOptions = DataTransferChannelOptions.Default
};
```

`SendTimeout` and `ReceiveTimeout` apply to one transport write or read. They do not replace association negotiation, release, or DIMSE response timeouts. Their default value is `Timeout.InfiniteTimeSpan`.

Use TLS by assigning an authenticator:

```csharp
using Aspose.Medical.Dicom.Network.Connection;

DicomNetworkConnectionOptions tlsClientConnection = connection with
{
    TlsAuthenticator = new TlsInitiatorAuthenticator
    {
        TargetHost = "pacs.example.test",
        Timeout = TimeSpan.FromSeconds(30)
    }
};
```

On a server, use `TlsAcceptorAuthenticator` and provide a certificate, certificate context, or selection callback:

```csharp
using System.Security.Cryptography.X509Certificates;
using Aspose.Medical.Dicom.Network.Connection;

X509Certificate2 serverCertificate =
    X509CertificateLoader.LoadPkcs12FromFile("server.pfx", "password");

DicomNetworkConnectionOptions tlsServerConnection = connection with
{
    TlsAuthenticator = new TlsAcceptorAuthenticator
    {
        TargetHost = "pacs.example.test",
        ServerCertificate = serverCertificate,
        ClientCertificateRequired = false,
        Timeout = TimeSpan.FromSeconds(30)
    }
};
```

## Configure Association Negotiation

`AssociationNegotiationOptions` describes AE titles, presentation contexts, extended negotiation, async operation limits, fallback text encoding, and association lifecycle timeouts.

```csharp
using System.Collections.Immutable;
using System.Text;
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Negotiation;

AssociationNegotiationOptions association = new AssociationNegotiationOptions
{
    Called = "SERVER_AE",
    Calling = "CLIENT_AE"
}
.WithMaxPduLength(262_144)
.WithFallbackEncoding(Encoding.ASCII)
.WithAsyncOperationWindow(new AsyncOperationsWindow(maxOperationsInvoked: 4, maxOperationsPerformed: 4))
.WithTimeouts(new AssociationTimeoutOptions
{
    OpenTimeout = TimeSpan.FromSeconds(5),
    ReleaseTimeout = TimeSpan.FromSeconds(10),
    CloseTimeout = TimeSpan.FromSeconds(10)
})
.WithPresentationContexts(new[]
{
    Presentation(Uid.Verification),
    Presentation(Uid.CTImageStorage),
    Presentation(Uid.StudyRootQueryRetrieveInformationModelFIND)
})
.WithExtendedNegotiation(ExtendedNegotiation.Create(
    Uid.StudyRootQueryRetrieveInformationModelFIND,
    CFindApplicationInfo.From(CFindOptions.RelationalQueries | CFindOptions.FuzzySemanticMatching)));

static PresentationContext Presentation(Uid abstractSyntax) => new()
{
    AbstractSyntax = abstractSyntax,
    Role = null,
    TransferSyntaxes = [TransferSyntax.ExplicitVrLittleEndian, TransferSyntax.ImplicitVrLittleEndian]
};
```

Use `Role = null` for default DICOM roles. Set `ServiceClassRole.User`, `ServiceClassRole.Provider`, or both when role selection is part of the association contract. The configured role is endpoint-local; server capabilities are mirrored to requestor-relative role-selection items during negotiation.

`ExtendedNegotiation.Create` adds SOP Class extended negotiation. `ExtendedNegotiation.CreateCommon` adds common extended negotiation with a service class and related general SOP classes.

Use the connection and association objects in client and server options:

```csharp
using Aspose.Medical.Dicom.Network;
using Aspose.Medical.Dicom.Network.Client;
using Aspose.Medical.Dicom.Network.Negotiation;
using Aspose.Medical.Dicom.Network.Protocol;
using Aspose.Medical.Dicom.Network.Server;

DicomNetworkClientOptions clientOptions = new()
{
    Connection = connection,
    AssociationNegotiation = association,
    Runtime = new DicomNetworkRuntimeOptions(),
    OperationResponseTimeout = TimeSpan.FromSeconds(30),
    MaxRequestsPerAssociation = 20,
    UserIdentity = new UserIdentityNegotiationDefinition
    {
        UserIdentityType = UserIdentityTypes.UsernameAndPasscode,
        PositiveResponseRequested = true,
        PrimaryField = "dicom-user",
        SecondaryField = "secret"
    }
};

DicomNetworkServerOptions serverOptions = new()
{
    Connection = connection,
    AssociationNegotiation = association,
    Runtime = new DicomNetworkRuntimeOptions(),
    OperationResponseTimeout = TimeSpan.FromSeconds(30),
    Backlog = 512,
    NoDelay = true,
    UserIdentityResponse = "accepted"
};
```

## Add Association Policy

Servers handle inbound association negotiation through `AssociationRequested`. If the configured `AssociationNegotiationOptions` are enough, no custom handler is required. Add a behavior when you need a small policy check before the default negotiation runs.

```csharp
using Aspose.Medical.Communication.Messages;
using Aspose.Medical.Communication.Processing;
using Aspose.Medical.Dicom.Network;
using Aspose.Medical.Dicom.Network.Protocol;
using Aspose.Medical.Dicom.Network.Server;

HashSet<string> allowedCallingAes = new(StringComparer.Ordinal)
{
    "CLIENT_AE",
    "ROUTER_AE"
};

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(serverOptions)
    .Use<AssociationRequested, AssociationRequestedResult>(new CallingAePolicy(allowedCallingAes))
    .Build();

sealed class CallingAePolicy : IRequestBehavior<AssociationRequested, AssociationRequestedResult>
{
    public CallingAePolicy(IReadOnlySet<string> allowedCallingAes)
        => _allowedCallingAes = allowedCallingAes;

    public ValueTask<AssociationRequestedResult> Handle(
        AssociationRequested request,
        SignalContinuation<ValueTask<AssociationRequestedResult>> next,
        CancellationToken cancellationToken)
    {
        string calling = request.Request.Calling.Trim(' ');
        if (!_allowedCallingAes.Contains(calling))
        {
            return ValueTask.FromResult<AssociationRequestedResult>(new AssociationRejectedResult
            {
                Result = AssociationRejectResult.Permanent,
                Source = AssociationRejectSource.ServiceUser,
                Reason = AssociationRejectReason.CallingAeNotRecognized
            });
        }

        return next(cancellationToken);
    }

    private readonly IReadOnlySet<string> _allowedCallingAes;
}
```

Return `AssociationAcceptedResult` only when you intentionally build the full `AssociationAccept` yourself. For most applications, let the default association handler negotiate presentation contexts, role selection, extended negotiation, user identity response, async operation window, and maximum PDU length from the server options.

## Build a C-ECHO Server and Client

Server request handlers receive DIMSE requests and return DIMSE responses. Client exchange handlers observe completed exchanges after responses arrive.

| Type | Method | Meaning |
| --- | --- | --- |
| `DicomNetworkClient` | `CreateBuilder(options)` | Creates a client builder |
| `DicomNetworkClientBuilder` | `Build()` | Creates a client with the configured processing graph |
| `DicomNetworkClient` | `QueueRequest(request)` | Adds one DIMSE request to the pending send queue |
| `DicomNetworkClient` | `QueueRequests(requests)` | Adds multiple DIMSE requests to the pending send queue |
| `DicomNetworkClient` | `SendAsync(cancellationToken)` | Starts or joins the active send operation |
| `DicomNetworkClient` | `StopAsync()` | Stops accepting more work and waits for the active send operation to finish |
| `DicomNetworkServer` | `CreateBuilder(options)` | Creates a server builder |
| `DicomNetworkServerBuilder` | `Build()` | Creates a server with the configured processing graph |
| `DicomNetworkServer` | `StartAsync(cancellationToken)` | Binds the listener and starts accepting associations |
| `DicomNetworkServer` | `StopAsync()` | Stops accepting connections and waits for active associations to finish |

```csharp
using System.Collections.Immutable;
using System.Net;
using Aspose.Medical.Communication.Notifications;
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Client;
using Aspose.Medical.Dicom.Network.Client.Handlers;
using Aspose.Medical.Dicom.Network.Connection;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Negotiation;
using Aspose.Medical.Dicom.Network.Server;
using Aspose.Medical.Dicom.Network.Server.Handlers;
using Aspose.Medical.Dicom.Tags;

const ushort Success = 0x0000;

IPEndPoint endpoint = new(IPAddress.Loopback, 104);

AssociationNegotiationOptions association = new AssociationNegotiationOptions
{
    Called = "SERVER_AE",
    Calling = "CLIENT_AE"
}.WithPresentationContext(new PresentationContext
{
    AbstractSyntax = Uid.Verification,
    Role = null,
    TransferSyntaxes = ImmutableArray.Create(TransferSyntax.ImplicitVrLittleEndian)
});

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(new DicomNetworkServerOptions
    {
        Connection = new DicomNetworkConnectionOptions { TargetHost = endpoint },
        AssociationNegotiation = association
    })
    .AddSingletonCEchoHandler(new EchoHandler())
    .Build();

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(new DicomNetworkClientOptions
    {
        Connection = new DicomNetworkConnectionOptions { TargetHost = endpoint },
        AssociationNegotiation = association
    })
    .AddSingletonCEchoHandler(new EchoObserver())
    .Build();

await server.StartAsync(CancellationToken.None);

try
{
    client.QueueRequest(new CEchoRequest());

    Task sending = await client.SendAsync(CancellationToken.None);
    await sending;
}
finally
{
    await client.StopAsync();
    await server.StopAsync();
}

sealed class EchoHandler : ICEchoRequestHandler
{
    public ValueTask<CEchoResponse> Handle(CEchoRequest request, CancellationToken cancellationToken)
        => ValueTask.FromResult(WithStatus(new CEchoResponse(), Success));

    private static CEchoResponse WithStatus(CEchoResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}

sealed class EchoObserver : ICEchoExchangeHandler
{
    public ValueTask Handle(UnaryExchangeNotification<CEchoRequest, CEchoResponse> exchange, CancellationToken cancellationToken)
    {
        Console.WriteLine($"C-ECHO status: 0x{exchange.Response.Status:X4}");
        return ValueTask.CompletedTask;
    }
}
```

`SendAsync` returns a `ValueTask<Task>`. Await the outer task to start or join the send operation, then await the returned `Task` to observe completion.

## Send C-STORE

`CStoreRequest` sends a DICOM dataset. The dataset must contain `SOPClassUID` and `SOPInstanceUID`. The transfer syntax argument describes the dataset syntax; additional syntaxes can be proposed for association negotiation.

```csharp
using Aspose.Medical.Dicom;
using System.Collections.Immutable;
using Aspose.Medical.Dicom.Network.Messages;

DicomFile file = DicomFile.Open("image.dcm");

CStoreRequest store = new(file.Dataset, file.MetaInfo.TransferSyntax)
{
    Priority = DimsePriority.Medium,
    AdditionalTransferSyntaxes = ImmutableArray.Create(
        TransferSyntax.ExplicitVrLittleEndian,
        TransferSyntax.ImplicitVrLittleEndian)
};

client.QueueRequest(store);

Task sending = await client.SendAsync(CancellationToken.None);
await sending;
```

Handle inbound C-STORE on the server:

```csharp
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Server.Handlers;
using Aspose.Medical.Dicom.Tags;

sealed class StoreHandler : ICStoreRequestHandler
{
    public async ValueTask<CStoreResponse> Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        DicomFile file = new(request.Dataset);
        await file.SaveAsync(GetPath(request), cancellationToken: cancellationToken);

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, 0x0000);
        return response;
    }

    private static string GetPath(CStoreRequest request)
        => Path.Combine("incoming", request.AffectedSopInstanceUid.Code + ".dcm");
}
```

## Query with C-FIND

`CFindRequest` is a stream request. The server can return pending responses followed by one terminal response. Pending statuses are `0xFF00` and `0xFF01`; other statuses are terminal.

```csharp
using Aspose.Medical.Dicom.Network.Messages;

CFindRequest find = CFindRequest.CreateStudyQuery(
    patientId: null,
    patientName: "DOE*",
    studyDateTime: null,
    accession: null,
    studyId: null,
    modalitiesInStudy: null,
    studyInstanceUid: null);

client.QueueRequest(find);
```

Register an exchange handler on the client to observe every response:

```csharp
using Aspose.Medical.Communication.Notifications;
using Aspose.Medical.Dicom.Network.Client.Handlers;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Tags;

sealed class FindObserver : ICFindExchangeHandler
{
    public ValueTask Handle(StreamExchangeNotification<CFindRequest, CFindResponse> exchange, CancellationToken cancellationToken)
    {
        if (exchange.Response.Identifier is not null)
            Console.WriteLine(exchange.Response.Identifier.GetSingleValueOrDefault(Tag.StudyInstanceUID, string.Empty));

        if (exchange.Response.IsTerminal)
            Console.WriteLine($"Final C-FIND status: 0x{exchange.Response.Status:X4}");

        return ValueTask.CompletedTask;
    }
}
```

Handle inbound C-FIND on the server:

```csharp
using System.Runtime.CompilerServices;
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Server.Handlers;
using Aspose.Medical.Dicom.Tags;

sealed class StudyFindHandler : ICFindRequestHandler
{
    public async IAsyncEnumerable<CFindResponse> Handle(
        CFindRequest request,
        [EnumeratorCancellation] CancellationToken cancellationToken)
    {
        foreach (Dataset match in FindStudies(request.Identifier))
        {
            cancellationToken.ThrowIfCancellationRequested();
            yield return WithStatus(new CFindResponse(match), 0xFF00);
        }

        yield return WithStatus(new CFindResponse(null), 0x0000);
        await Task.CompletedTask;
    }

    private static IEnumerable<Dataset> FindStudies(Dataset identifier)
    {
        Dataset match = new();
        match.AddOrUpdate(Tag.PatientName, "DOE^JANE");
        match.AddOrUpdate(Tag.StudyInstanceUID, "1.2.840.113619.2.55.3.604688433.123");
        yield return match;
    }

    private static CFindResponse WithStatus(CFindResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}
```

## Retrieve with C-GET and C-MOVE

`CGetRequest` and `CMoveRequest` are also stream requests. Their responses expose `Remaining`, `Completed`, `Warnings`, and `Failures` counters.

```csharp
using Aspose.Medical.Dicom.Network.Messages;

CGetRequest get = CGetRequest.CreateForStudy("1.2.840.113619.2.55.3.604688433.123");
CMoveRequest move = CMoveRequest.CreateForStudy("DEST_AE", Uid.Parse("1.2.840.113619.2.55.3.604688433.123"));

client.QueueRequests(new IDicomRequest[] { get, move });
```

### Sub-Operations on the Active Association

A handler receives an `IDicomRequestContext` through the request object. When `request.Context.CanInvokeOperations` is `true`, the handler is running inside an active association and can send a unary DIMSE request back over that same association with `InvokeAsync`.

Use this for same-association sub-operations, such as a C-GET SCP sending C-STORE requests to the C-GET SCU. Register handlers for inbound sub-operations on the endpoint that can receive them. For example, a C-GET client needs a C-STORE request handler on its client builder if the server will send C-STORE sub-operations during the C-GET exchange.

```csharp
using Aspose.Medical.Communication.Messages;
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Client;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Tags;

DicomNetworkClient client = DicomNetworkClient
    .CreateBuilder(clientOptions)
    .AddRequestHandler<CStoreRequest, CStoreResponse>(new IncomingStoreHandler())
    .Build();

sealed class IncomingStoreHandler : IRequestHandler<CStoreRequest, CStoreResponse>
{
    public ValueTask<CStoreResponse> Handle(CStoreRequest request, CancellationToken cancellationToken)
    {
        Save(request.Dataset);

        CStoreResponse response = new();
        response.Command.AddOrUpdate(Tag.Status, 0x0000);
        return ValueTask.FromResult(response);
    }

    private static void Save(Dataset instance)
    {
    }
}
```

```csharp
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Errors;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Server.Handlers;
using Aspose.Medical.Dicom.Tags;

sealed class GetHandler : ICGetRequestHandler
{
    public async IAsyncEnumerable<CGetResponse> Handle(CGetRequest request, CancellationToken cancellationToken)
    {
        if (!request.Context.CanInvokeOperations)
            throw new DicomNetworkException("C-GET requires an active association for C-STORE sub-operations.");

        foreach (Dataset instance in ResolveInstances(request.Dataset))
        {
            CStoreRequest store = new(instance, TransferSyntax.ExplicitVrLittleEndian);
            CStoreResponse storeResponse = await request.Context.InvokeAsync(store, cancellationToken);

            CGetResponse pending = WithStatus(new CGetResponse(), 0xFF00);
            pending.Completed = storeResponse.Status == 0x0000 ? 1 : 0;
            pending.Failures = storeResponse.Status == 0x0000 ? 0 : 1;
            yield return pending;
        }

        yield return WithStatus(new CGetResponse(), 0x0000);
    }

    private static IEnumerable<Dataset> ResolveInstances(Dataset keys) => Array.Empty<Dataset>();

    private static CGetResponse WithStatus(CGetResponse response, ushort status)
    {
        response.Command.AddOrUpdate(Tag.Status, status);
        return response;
    }
}
```

`Context.InvokeAsync` waits for the terminal response to the invoked request. That wait is controlled by the endpoint's `OperationResponseTimeout`. If `CanInvokeOperations` is `false`, the request is not attached to an active association and cannot invoke sub-operations.

## Use N-Service Requests

N-service requests model normalized DICOM operations. They are unary requests and have matching response classes:

| Operation | Request | Response |
| --- | --- | --- |
| N-EVENT-REPORT | `NEventReportRequest` | `NEventReportResponse` |
| N-GET | `NGetRequest` | `NGetResponse` |
| N-SET | `NSetRequest` | `NSetResponse` |
| N-ACTION | `NActionRequest` | `NActionResponse` |
| N-CREATE | `NCreateRequest` | `NCreateResponse` |
| N-DELETE | `NDeleteRequest` | `NDeleteResponse` |

Example N-GET request:

```csharp
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Tags;

NGetRequest request = new(
    requestedSopClassUid: Uid.BasicFilmSessionSOPClass,
    requestedSopInstanceUid: Uid.Parse("1.2.840.10008.5.1.1.1"),
    attributeIdentifiers: [Tag.NumberOfCopies, Tag.PrintPriority]);

client.QueueRequest(request);
```

Example N-GET handler:

```csharp
using Aspose.Medical.Dicom;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Server.Handlers;
using Aspose.Medical.Dicom.Tags;

sealed class FilmSessionGetHandler : INGetRequestHandler
{
    public ValueTask<NGetResponse> Handle(NGetRequest request, CancellationToken cancellationToken)
    {
        Dataset attributes = new();
        attributes.AddOrUpdate(Tag.NumberOfCopies, "1");
        attributes.AddOrUpdate(Tag.PrintPriority, "MED");

        NGetResponse response = new(attributes);
        response.Command.AddOrUpdate(Tag.Status, 0x0000);
        return ValueTask.FromResult(response);
    }
}
```

## Register Handlers and Behaviors

Use operation-specific server request handlers to answer DIMSE requests received by a server association. Use operation-specific client exchange handlers to observe completed exchanges for requests sent by the client.

Both builders also expose generic request, stream, behavior, and notification registrations. These are endpoint registrations: register them on the endpoint that may receive or publish that route, including sub-operations on an association opened by the other side. A client-side C-STORE request handler for C-GET sub-operations is a normal use of the client builder, not a server-only concern.

| Operation kind | Server handler | Client exchange handler |
| --- | --- | --- |
| C-CANCEL | `ICCancelRequestHandler` | `ICCancelExchangeHandler` |
| C-ECHO | `ICEchoRequestHandler` | `ICEchoExchangeHandler` |
| C-STORE | `ICStoreRequestHandler` | `ICStoreExchangeHandler` |
| C-FIND | `ICFindRequestHandler` | `ICFindExchangeHandler` |
| C-GET | `ICGetRequestHandler` | `ICGetExchangeHandler` |
| C-MOVE | `ICMoveRequestHandler` | `ICMoveExchangeHandler` |
| N-EVENT-REPORT | `INEventReportRequestHandler` | `INEventReportExchangeHandler` |
| N-GET | `INGetRequestHandler` | `INGetExchangeHandler` |
| N-SET | `INSetRequestHandler` | `INSetExchangeHandler` |
| N-ACTION | `INActionRequestHandler` | `INActionExchangeHandler` |
| N-CREATE | `INCreateRequestHandler` | `INCreateExchangeHandler` |
| N-DELETE | `INDeleteRequestHandler` | `INDeleteExchangeHandler` |

Builder registration groups:

| Purpose | Server builder | Client builder |
| --- | --- | --- |
| Operation-specific shared handler | `AddSingletonCEchoHandler`, `AddSingletonCStoreHandler`, `AddSingletonCFindHandler`, `AddSingletonCGetHandler`, `AddSingletonCMoveHandler`, `AddSingletonCCancelHandler`, `AddSingletonN*Handler` | same names, but handler type is the client exchange handler |
| Operation-specific handler factory | `AddCEchoHandlerFactory`, `AddCStoreHandlerFactory`, `AddCFindHandlerFactory`, `AddCGetHandlerFactory`, `AddCMoveHandlerFactory`, `AddCCancelHandlerFactory`, and N-service factory shortcuts where available | same names, but factory type is the client exchange handler factory |
| Generic unary request handler | `AddRequestHandler<TRequest, TResponse>(IRequestHandler<TRequest, TResponse>)` and `AddRequestHandlerFactory<TRequest, TResponse>(IRequestHandlerFactory<TRequest, TResponse>)` | `AddRequestHandler<TRequest, TResponse>(IRequestHandler<TRequest, TResponse>)` |
| Generic stream request handler | `AddRequestHandler<TRequest, TResponse>(IStreamRequestHandler<TRequest, TResponse>)` and factory overload | `AddStreamRequestHandler<TRequest, TResponse>` and factory overload |
| Unary request behavior | `Use<TRequest, TResponse>(IRequestBehavior<TRequest, TResponse>)`, factory overload, or `Use(IOpenRequestBehaviorFactory)` | same |
| Stream request behavior | `Use<TRequest, TResponse>(IStreamRequestBehavior<TRequest, TResponse>)`, factory overload, or `Use(IOpenStreamRequestBehaviorFactory)` | same |
| Notification handler | `AddNotificationHandler<TNotification>`, factory overload, or `AddOpenNotificationHandlerFactory` | same |

***Instance registrations reuse the same object for every matching dispatch. Use them for stateless, immutable, or explicitly thread-safe handlers and behaviors.***

Factory registrations call `Create()` for each route execution. Use factories for request-local state, per-execution scopes, or non-thread-safe collaborators. Use generic factory overloads when a shortcut does not match the route you need. The framework creates the instance; it does not make a shared handler thread-safe.

```csharp
using Aspose.Medical.Communication.Messages;
using Aspose.Medical.Communication.Processing;
using Aspose.Medical.Dicom.Network.Messages;
using Aspose.Medical.Dicom.Network.Server;
using Aspose.Medical.Dicom.Network.Server.Handlers;

DicomNetworkServerBuilder builder = DicomNetworkServer.CreateBuilder(serverOptions);

builder.AddSingletonCEchoHandler(new StatelessEchoHandler());
builder.AddCEchoHandlerFactory(new EchoHandlerFactory());

builder.Use<CEchoRequest, CEchoResponse>(new LoggingBehavior());
builder.Use<CEchoRequest, CEchoResponse>(new LoggingBehaviorFactory());

sealed class EchoHandlerFactory : ICEchoRequestHandlerFactory
{
    public ISignalHandler<CEchoRequest, ValueTask<CEchoResponse>> Create()
        => new StatefulEchoHandler();
}

sealed class LoggingBehavior : IRequestBehavior<CEchoRequest, CEchoResponse>
{
    public async ValueTask<CEchoResponse> Handle(
        CEchoRequest request,
        SignalContinuation<ValueTask<CEchoResponse>> next,
        CancellationToken cancellationToken)
    {
        Console.WriteLine($"C-ECHO message id {request.MessageId}");
        return await next(cancellationToken);
    }
}

sealed class LoggingBehaviorFactory : IRequestBehaviorFactory<CEchoRequest, CEchoResponse>
{
    public ISignalBehavior<CEchoRequest, ValueTask<CEchoResponse>> Create()
        => new LoggingBehavior();
}
```

Open factories apply the same pattern to every compatible request, stream request, or notification route:

```csharp
builder.Use(new AuditAllUnaryRequestsFactory());
builder.Use(new AuditAllStreamRequestsFactory());
builder.AddOpenNotificationHandlerFactory(new AuditAllNotificationsFactory());
```

Minimal open notification factory:

```csharp
using Aspose.Medical.Communication;
using Aspose.Medical.Communication.Notifications;

sealed class AuditAllNotificationsFactory : IOpenNotificationHandlerFactory
{
    public INotificationHandler<TNotification> Create<TNotification>()
        where TNotification : INotification
        => new AuditNotificationHandler<TNotification>();
}

sealed class AuditNotificationHandler<TNotification> : INotificationHandler<TNotification>
    where TNotification : INotification
{
    public ValueTask Handle(TNotification notification, CancellationToken cancellationToken)
    {
        Console.WriteLine($"Notification: {typeof(TNotification).Name}");
        return ValueTask.CompletedTask;
    }
}
```

## Observe Network Notifications

Notification handlers can observe association lifecycle changes, aborts, releases, request timeouts, and completed DIMSE exchanges.

| Notification | Published when | Payload |
| --- | --- | --- |
| `AssociationAcceptedNotification` | Association negotiation succeeds | `NegotiatedAssociation` |
| `AssociationRejectedNotification` | A-ASSOCIATE-RJ is sent or received | `Result`, `Source`, `Reason` |
| `AssociationReleasedNotification` | Association release completes | none |
| `AssociationReleaseRequestedNotification` | Peer requests release with A-RELEASE-RQ | none |
| `AssociationEstablishmentTimedOutNotification` | Association open timer expires before negotiation completes | none |
| `AssociationReleaseResponseTimedOutNotification` | Release timer expires while waiting for A-RELEASE-RP | none |
| `AssociationAbortReceivedNotification` | Peer sends A-ABORT or invalid PDU implies abort handling | `Source`, `Reason` |
| `AssociationAbortSentNotification` | Local endpoint sends A-ABORT | `Source`, `Reason` |
| `RequestTimedOutNotification` | A locally sent DIMSE request reaches `OperationResponseTimeout` | `Request` |
| `UnaryExchangeNotification<TRequest,TResponse>` | Unary request completes | request and response |
| `StreamExchangeNotification<TRequest,TResponse>` | Stream request receives a response | request and current response |

```csharp
using Aspose.Medical.Communication.Notifications;
using Aspose.Medical.Dicom.Network;
using Aspose.Medical.Dicom.Network.Server;

AssociationAudit audit = new();

DicomNetworkServer server = DicomNetworkServer
    .CreateBuilder(serverOptions)
    .AddNotificationHandler<AssociationAcceptedNotification>(audit)
    .AddNotificationHandler<AssociationRejectedNotification>(audit)
    .AddNotificationHandler<AssociationAbortReceivedNotification>(audit)
    .Build();

sealed class AssociationAudit :
    INotificationHandler<AssociationAcceptedNotification>,
    INotificationHandler<AssociationRejectedNotification>,
    INotificationHandler<AssociationAbortReceivedNotification>
{
    public ValueTask Handle(AssociationAcceptedNotification notification, CancellationToken cancellationToken)
    {
        Console.WriteLine($"Accepted with {notification.NegotiatedAssociation.PresentationContexts.Count} contexts");
        return ValueTask.CompletedTask;
    }

    public ValueTask Handle(AssociationRejectedNotification notification, CancellationToken cancellationToken)
    {
        Console.WriteLine($"Rejected: {notification.Result}/{notification.Source}/{notification.Reason}");
        return ValueTask.CompletedTask;
    }

    public ValueTask Handle(AssociationAbortReceivedNotification notification, CancellationToken cancellationToken)
    {
        Console.WriteLine($"Abort received: {notification.Source}/{notification.Reason}");
        return ValueTask.CompletedTask;
    }
}
```

Use `AssociationAcceptedNotification.NegotiatedAssociation` to inspect the effective presentation contexts, accepted transfer syntaxes, local service-class roles, extended negotiation results, async operation window, maximum PDU length, and user-identity server response.

## Configure Timeouts

Timeouts are layered. Configure them together instead of using one timeout for every network phase.

```csharp
using Aspose.Medical.Dicom.Network.Client;
using Aspose.Medical.Dicom.Network.Connection;
using Aspose.Medical.Dicom.Network.Negotiation;

DicomNetworkConnectionOptions connection = new()
{
    TargetHost = endpoint,
    ConnectTimeout = TimeSpan.FromSeconds(10),
    SendTimeout = TimeSpan.FromSeconds(15),
    ReceiveTimeout = TimeSpan.FromSeconds(15),
    CloseTimeout = TimeSpan.FromSeconds(10)
};

AssociationNegotiationOptions association = new AssociationNegotiationOptions
{
    Called = "SERVER_AE",
    Calling = "CLIENT_AE"
}.WithTimeouts(new AssociationTimeoutOptions
{
    OpenTimeout = TimeSpan.FromSeconds(5),
    ReleaseTimeout = TimeSpan.FromSeconds(10),
    CloseTimeout = TimeSpan.FromSeconds(10)
});

DicomNetworkClientOptions options = new()
{
    Connection = connection,
    AssociationNegotiation = association,
    OperationResponseTimeout = TimeSpan.FromSeconds(30),
    MaxRequestsPerAssociation = 25
};
```

| Timeout | Owner | Meaning |
| --- | --- | --- |
| `ConnectTimeout` | `DicomNetworkConnectionOptions` | TCP connect budget before the association transport exists |
| `TlsInitiatorAuthenticator.Timeout`, `TlsAcceptorAuthenticator.Timeout` | TLS authenticator | TLS authentication budget |
| `OpenTimeout` | `AssociationTimeoutOptions` | Association negotiation budget after the transport is open |
| `OperationResponseTimeout` | Client/server options | Time waiting for the next expected response to a locally invoked DIMSE request |
| `ReleaseTimeout` | `AssociationTimeoutOptions` | Time waiting for `A-RELEASE-RP` after the local side sends `A-RELEASE-RQ` |
| `AssociationTimeoutOptions.CloseTimeout` | Association state machine | Time waiting for transport close after a local reject, release response, or abort |
| `DicomNetworkConnectionOptions.CloseTimeout` | Transport | Time waiting for transport worker loops to finish during connection shutdown |
| `SendTimeout` / `ReceiveTimeout` | Transport | One write or read operation |

`OperationResponseTimeout` restarts after each pending response. For `C-FIND`, `C-GET`, and `C-MOVE`, that means every pending response extends the wait for the next response. If it expires, the request is timed out, `RequestTimedOutNotification` is published, and the association is aborted.

When the application stops a send operation or a peer asks to release an association, the endpoint stops accepting new work and tries to complete already active work. The active work is bounded by the operation it is doing:

- if the endpoint is waiting for a response to a request it invoked, `OperationResponseTimeout` applies;
- if it is writing a response, `SendTimeout` applies to the transport write;
- if it is waiting for inbound bytes, `ReceiveTimeout` applies to that read;
- after the local side sends `A-RELEASE-RQ`, `ReleaseTimeout` applies while waiting for `A-RELEASE-RP`.

Use `Timeout.InfiniteTimeSpan` only when another application-level cancellation path controls the same wait.

## Handle Errors

Networking failures derive from `DicomNetworkException`. Catch specific exceptions when the recovery path depends on the protocol outcome.

| Exception | Use |
| --- | --- |
| `AssociationRejectedException` | Association was rejected; read `Result`, `IssuedRejection`, and `Reason` |
| `AssociationAbortException` | Association was aborted; read `AbortSource` and `Reason` |
| `AssociationReleasedException` | Association was released while an operation still needed it |
| `AsyncOperationsWindowExceededException` | Peer exceeded the negotiated async-operation limit |
| `InvalidAssociationTransitionException` | Association state machine rejected an invalid lifecycle transition |
| `PduReadException` | PDU data was invalid or unexpected; read `Reason` for the abort diagnostic |
| `TimeoutException` | Transport, TLS, association, or operation timeout fired |

```csharp
using Aspose.Medical.Dicom.Network.Errors;

try
{
    Task sending = await client.SendAsync(CancellationToken.None);
    await sending;
}
catch (AssociationRejectedException ex)
{
    Console.WriteLine($"Association rejected: {ex.Result}/{ex.IssuedRejection}/{ex.Reason}");
}
catch (AssociationAbortException ex)
{
    Console.WriteLine($"Association aborted: {ex.AbortSource}/{ex.Reason}");
}
catch (TimeoutException ex)
{
    Console.WriteLine(ex.Message);
}
catch (DicomNetworkException ex)
{
    Console.WriteLine(ex.Message);
}
```

`AssociationReleasedException` indicates that the association was released while an operation was still using it. `AsyncOperationsWindowExceededException` indicates that the negotiated async-operation limit was exceeded. `PduReadException` reports invalid or unexpected PDU data together with the abort reason.

## Association Models

Normal client and server code usually works with `AssociationNegotiationOptions`, handlers, and notifications. Use these models when inspecting an established association, writing custom association policy, or interpreting association-control outcomes.

Established association state:

| Type | Role |
| --- | --- |
| `EffectiveAssociation` | Endpoint-local association contract after negotiation succeeds; available to both client and server endpoints |
| `EffectivePresentationContext` | Accepted presentation context, transfer syntax, and local service-class role on an established association |

Association control:

| Type or surface | Role |
| --- | --- |
| `AssociationRequested` | Request sent through the server pipeline when an inbound A-ASSOCIATE-RQ needs an accept/reject decision |
| `AssociationRequestedResult` | Base response type for association policy handlers |
| `AssociationAcceptedResult` | Accept result returned by an association policy handler |
| `AssociationRejectedResult` | Reject result returned by an association policy handler |
| `AssociationRequest` | A-ASSOCIATE-RQ data: AE titles, maximum PDU length, implementation identity, async limits, presentation contexts, roles, extended negotiation, common extended negotiation, and user identity |
| `AssociationAccept` | A-ASSOCIATE-AC data: accepted AE titles, maximum PDU length, implementation identity, async limits, user-identity response, presentation-context results, accepted roles, and extended-negotiation results |
| `PresentationContextDefinition` | Proposed presentation context with abstract syntax and transfer syntax candidates |
| `PresentationContextResult` | Accepted or rejected presentation context result |
| `PresentationContextReason` | Presentation-context accept/reject reason |
| `RoleSelectionDefinition` | Proposed service-class role for a SOP Class |
| `RoleSelectionResult` | Accepted service-class role for a SOP Class |
| `ExtendedNegotiationDefinition` | Proposed SOP Class extended-negotiation application information |
| `ExtendedNegotiationResult` | Accepted SOP Class extended-negotiation application information |
| `CommonExtendedNegotiationDefinition` | Proposed common extended negotiation: SOP Class, service class, and related general SOP Classes |
| `UserIdentityNegotiationDefinition` | Proposed user identity type, positive-response flag, and identity fields |
| `UserIdentityTypes` | User identity negotiation type |
| `AssociationRejectResult`, `AssociationRejectSource`, `AssociationRejectReason` | Rejection details used by `AssociationRejectedResult`, `AssociationRejectedNotification`, and `AssociationRejectedException` |
| `AssociationAbortSource`, `AssociationAbortReason` | Abort details used by abort notifications and `AssociationAbortException` |
| A-RELEASE-RQ / A-RELEASE-RP | Release control PDUs; exposed through `AssociationReleaseRequestedNotification`, `AssociationReleasedNotification`, `AssociationReleaseResponseTimedOutNotification`, and `AssociationReleasedException` |

Key public fields:

| Type | Public fields |
| --- | --- |
| `AssociationRequested` | `Request` |
| `AssociationRequest` | `Called`, `Calling`, `MaxPduLength`, `ImplementationClass`, `ImplementationVersionName`, `AsyncOperationsLimits`, `PresentationContexts`, `RoleSelections`, `ExtendedNegotiations`, `CommonExtendedNegotiations`, `UserIdentityNegotiation` |
| `AssociationAccept` | `Called`, `Calling`, `MaxPduLength`, `ImplementationClass`, `ImplementationVersionName`, `AsyncOperationsLimits`, `UserIdentityServerResponse`, `PresentationContexts`, `RoleSelections`, `ExtendedNegotiations` |
| `EffectiveAssociation` | `MaxPduLength`, `OperationsWindow`, `PresentationContexts`, `ExtendedNegotiations`, `UserIdentityServerResponse` |
| `EffectivePresentationContext` | `Id`, `AbstractSyntax`, `TransferSyntax`, `LocalRole` |
| `PresentationContextDefinition` | `Id`, `AbstractSyntax`, `TransferSyntaxes` |
| `PresentationContextResult` | `Id`, `Reason`, `AcceptedSyntax` |
| `RoleSelectionDefinition`, `RoleSelectionResult` | `AbstractSyntax`, `Role` |
| `ExtendedNegotiationDefinition`, `ExtendedNegotiationResult` | `SopClassUid`, `ApplicationInfo` |
| `CommonExtendedNegotiationDefinition` | `SopClassUid`, `ServiceClassUid`, `RelatedGeneralSopClassUids` |
| `UserIdentityNegotiationDefinition` | `UserIdentityType`, `PositiveResponseRequested`, `PrimaryField`, `SecondaryField` |
| `AssociationAcceptedResult` | `NegotiatedAssociation` |
| `AssociationRejectedResult` | `Result`, `Source`, `Reason` |

Read the effective association from `AssociationAcceptedNotification`:

```csharp
using Aspose.Medical.Communication.Notifications;
using Aspose.Medical.Dicom.Network;

sealed class AcceptedAssociationLog : INotificationHandler<AssociationAcceptedNotification>
{
    public ValueTask Handle(AssociationAcceptedNotification notification, CancellationToken cancellationToken)
    {
        foreach (var context in notification.NegotiatedAssociation.PresentationContexts.Values)
            Console.WriteLine($"{context.Id}: {context.AbstractSyntax} via {context.TransferSyntax}, local role {context.LocalRole}");

        Console.WriteLine($"Max PDU: {notification.NegotiatedAssociation.MaxPduLength}");
        Console.WriteLine($"Async window: {notification.NegotiatedAssociation.OperationsWindow}");
        return ValueTask.CompletedTask;
    }
}
```

Interpret rejection details from a notification or exception:

```csharp
using Aspose.Medical.Dicom.Network;
using Aspose.Medical.Dicom.Network.Protocol;
using Aspose.Medical.Communication.Notifications;

sealed class AssociationPolicy : INotificationHandler<AssociationRejectedNotification>
{
    public ValueTask Handle(AssociationRejectedNotification notification, CancellationToken cancellationToken)
    {
        if (notification.Result == AssociationRejectResult.Transient)
            Console.WriteLine("Retry may be possible later.");

        return ValueTask.CompletedTask;
    }
}
```
