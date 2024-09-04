# Platform State of The Union
## Apple Inteligence
Apple Inteligence will bring powerful generative models to Apple platforms, such as iOS, iPadOS, watchOS and visionOS 
 They target to run all machine learning tasks on device, taking advantage of Apple Silicon. The reason for trying to run almost everything on-device, is that it delivers low-latency and better user experience and privacy to users.

Apple Intelligence starts with our on-device Foundation model, a highly capable LLM. Powerful enough for awesome experince and small enough to run on-device.

Three key challenges to solve:
- Specialization: Specialize to be great for many different tasks
- Size: Making it small enough to be on-device
- Perfomance: Deliver the best possible inference perfomance and energy efficience

<ADD EXPLANATION HERE>

Apple Intelligence is powerful, intuitive, and integrated language and diffusion models that deliver perfomance and can fit on your device
However, we have feature that require larger models to reason over more complex data. So, for running large foundation models, it was extended Apple Intelligence to the cloud with Private Cloud Compute

Since this LLM gets user's personal data, it was needed to extend the privacy that is done on devices to servers.
Private Cloud Compute is designated for processing AI privately. It runs on a new OS using a hardened subset of the foundations of iOS. For guarating the privacy

Apple Silicon powers a lot of features for the Private Cloud Compute:
- *Secure Enclave* to protect critical encryption keys.
- *Secure Boot* ensures the OS is signed and verified just like on iOS.
- *Trusted Execeution Monitor* makes sure that only signed and verified code runs.
- *Attestation* enables a user's device to securely verify the identity and configuration of a Private Cloud Compute cluster before sending a request to the server



