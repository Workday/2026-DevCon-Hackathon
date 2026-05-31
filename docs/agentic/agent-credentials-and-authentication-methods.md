# Overview

To securely connect an external agent to the **Agent Gateway**, you must configure authentication based on the skills defined for the agent. Workday uses the OAuth 2.0 framework to authorize agent access to Workday APIs and ensure that requests route through the correct regional gateway endpoints.

### Authentication by Agent Skill Type

The authentication requirements and underlying OAuth structures differ depending on whether the agent uses delegate or ambient skills. Workday aligns these tenanted OAuth structures during the registration process. You manage the resulting credentials in the Connectivity tab of the agent profile.

* **Delegate Skills**: These agents act on behalf of a specific Workday user. This method is used for agents that require user-specific security permissions and session-based interactions.
* **Ambient Skills**: These agents operate independently of a specific user session and are governed by their own security permissions. Workday validates that all APIs used by ambient skills are secured to domains that allow ambient access. An API is permitted if the domain’s **Allowed Security Group Types** field meets any of these criteria:
  * The field is **blank** (default).
  * The field includes **Ambient Skill Security Group**.
  * The field includes **Unconstrained Groups**.

### Comparison of Authentication Details

While both methods use the same regional authorization and token endpoint patterns, they utilize distinct client materials within the ASOR.

|Feature|Delegate Skills|Ambient Skills|
| --- | --- | --- |
|**OAuth Flow**|Authorization Code Grant|x509 Key Association|
|**Primary Credential**|Client ID and Client Secret|x509 Public Key|
|**Activation Requirement**|Redirect URI: Required for activation.|x509 Association: Required for activation.|
|**Credential Visibility**|Regional OAuth Endpoints and Delegate Client ID are visible immediately.|Regional OAuth Endpoints are visible immediately. Ambient Client ID and ASU Username display once the x509 key is associated.|