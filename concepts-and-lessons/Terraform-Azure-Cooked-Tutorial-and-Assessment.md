# ☁️ Terraform + Azure — Turning Architecture Into Executable Infrastructure

## Cooked App Tutorial + Assessment

---

# 🌍 The Story So Far

Cooked already has an application shape:

```text
USER
↓
React Frontend
↓
Java / Spring Boot Backend
↓
PostgreSQL
```

It also needs:

```text
image storage
secrets
AI-processing capability
logging / monitoring
controlled access
controlled network communication
```

The moment we ask **where these things live, who can access them, how they connect, and how another engineer could reproduce them**, we have moved from application code into infrastructure architecture.

Terraform exists to make that infrastructure architecture explicit.

---

# 🧠 1️⃣ Before Terraform — Infrastructure Lived in People's Heads

Imagine building Cooked manually in Azure:

```text
open Azure Portal
↓
create Resource Group
↓
create Storage Account
↓
create database
↓
configure networking
↓
create app hosting
↓
configure secrets
↓
configure monitoring
```

Eventually the system may work.

But where does the architecture now live?

```text
Azure
+
documentation
+
the engineer's memory
```

Ask another engineer to reproduce it exactly and the answer may be:

```text
maybe
```

Terraform solves that problem by making the desired infrastructure explicit in code.

---

# 📖 2️⃣ Terraform Is Infrastructure as Code

Instead of saying:

```text
Create a Resource Group called cooked-dev-rg in UK South.
```

we can write:

```hcl
resource "azurerm_resource_group" "cooked" {
  name     = "cooked-dev-rg"
  location = "UK South"
}
```

The important idea is not the punctuation.

It is the transformation:

```text
ARCHITECTURAL INTENT
↓
TEXTUAL SPECIFICATION
↓
INFRASTRUCTURE
```

Terraform is a way of representing infrastructure so that it becomes:

```text
reviewable
repeatable
versionable
comparable
automatable
```

---

# 🧬 3️⃣ Terraform Is Spec Descent

We have already learned this pattern:

```text
HIGH-LEVEL INTENT
↓
INVARIANT
↓
ARCHITECTURAL DECISION
↓
IMPLEMENTATION
```

Terraform lives near the bottom.

For Cooked:

```text
INVARIANT

Fridge images must be stored durably.
↓
CAPABILITY

Object storage.
↓
AZURE DECISION

Blob Storage.
↓
TERRAFORM

azurerm_storage_account
+
azurerm_storage_container
```

So do not begin with Terraform resource names.

Begin with:

> **What must remain true?**

Then descend.

---

# 🎯 4️⃣ Terraform Is Declarative

Terraform is not primarily:

```text
Step 1: click this
Step 2: create this
Step 3: connect this
```

Instead we describe:

> **What should exist.**

Example:

```hcl
resource "azurerm_resource_group" "cooked" {
  name     = "cooked-dev-rg"
  location = "UK South"
}
```

We are declaring a desired state:

```text
A Resource Group
named cooked-dev-rg
should exist
in UK South.
```

Terraform works out which operations are required to move the infrastructure toward that desired state.

---

# 🔁 5️⃣ The Core Terraform Loop

Terraform compares:

```text
CURRENT / KNOWN STATE
+
DESIRED CONFIGURATION
↓
DIFFERENCE
↓
PLAN
```

That is the heart of Terraform.

A useful analogy is:

```text
OLD STATE
+
NEW INTENT
↓
CALCULATED TRANSITION
```

Terraform is therefore a machine for reasoning about infrastructure change.

---

# 🔮 6️⃣ What Is `terraform plan`?

Suppose Azure currently has nothing for Cooked.

Your Terraform says:

```text
Resource Group
Storage Account
Blob Container
Virtual Network
Database
```

Terraform can calculate:

```text
+ create Resource Group
+ create Storage Account
+ create Blob Container
+ create Virtual Network
+ create Database
```

That proposed change is the **plan**.

Run:

```bash
terraform plan
```

The plan answers:

> **If Terraform were allowed to act, what would it attempt to create, change or destroy?**

For this course, that reasoning is more important than actually deploying the resources.

---

# ⚠️ 7️⃣ Plan Is Not Apply

```text
terraform plan
↓
SHOW what would change
```

```text
terraform apply
↓
MAKE the change
```

For this assessment:

> **Do not run `terraform apply` unless specifically authorised.**

We are assessing infrastructure understanding, not Azure spend.

---

# 🧰 8️⃣ The Basic Workflow

```text
WRITE
↓
terraform fmt
↓
terraform init
↓
terraform validate
↓
terraform plan
↓
terraform apply
```

For us, stop before apply.

```bash
terraform fmt
```

Formats the files.

```bash
terraform init
```

Initialises the project and downloads providers.

```bash
terraform validate
```

Checks the Terraform configuration.

```bash
terraform plan
```

Calculates the proposed infrastructure changes.

---

# 🔌 9️⃣ Providers — Terraform Needs an Adapter

Terraform itself does not know how Azure works.

```text
TERRAFORM
↓
AZURE PROVIDER
↓
AZURE APIs
```

A basic provider configuration:

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

Meaning:

```text
This Terraform project uses
the AzureRM provider
to describe Azure resources.
```

---

# 🧩 1️⃣0️⃣ Resources — Things Terraform Manages

A Terraform resource is something Terraform manages.

Examples:

```text
Resource Group
Virtual Network
Storage Account
PostgreSQL server
Key Vault
Application host
```

Example:

```hcl
resource "azurerm_resource_group" "cooked" {
  name     = "cooked-dev-rg"
  location = "UK South"
}
```

Read it as:

```text
RESOURCE TYPE
azurerm_resource_group

LOCAL TERRAFORM NAME
cooked

AZURE NAME
cooked-dev-rg
```

---

# 🏷️ 1️⃣1️⃣ Terraform Name vs Azure Name

This distinction matters.

```hcl
resource "azurerm_resource_group" "cooked" {
  name = "cooked-dev-rg"
}
```

Terraform calls the object:

```text
azurerm_resource_group.cooked
```

Azure sees:

```text
cooked-dev-rg
```

Later we can reference it:

```hcl
azurerm_resource_group.cooked.name
```

Meaning:

```text
take the resource group
Terraform knows as "cooked"
and use its Azure name
```

---

# 🕸️ 1️⃣2️⃣ References Create Relationships

Now add a Storage Account:

```hcl
resource "azurerm_storage_account" "images" {
  name                     = "cookeddevimages"
  resource_group_name      = azurerm_resource_group.cooked.name
  location                 = azurerm_resource_group.cooked.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

Notice:

```hcl
resource_group_name = azurerm_resource_group.cooked.name
```

That reference creates a relationship:

```text
RESOURCE GROUP
↓
STORAGE ACCOUNT
```

Terraform can infer that the Storage Account depends on the Resource Group.

---

# 🧠 1️⃣3️⃣ Terraform Builds a Dependency Graph

Terraform is not simply reading files from top to bottom.

It builds a graph.

```text
Resource Group
├── Storage
├── VNet
├── Key Vault
├── PostgreSQL
└── App Hosting
```

Then:

```text
Storage Account
↓
Blob Container
```

and perhaps:

```text
VNet
↓
Subnet
↓
Private Endpoint
↓
Database
```

This is why Terraform fits our systems-thinking approach so well.

Infrastructure is already a network of dependencies.

Terraform makes that network explicit.

---

# 📂 1️⃣4️⃣ File Names Are for Humans

You might use:

```text
providers.tf
variables.tf
main.tf
network.tf
database.tf
outputs.tf
```

Terraform treats `.tf` files in the same directory as one configuration.

So:

```text
FILE ORDER
≠
DEPENDENCY ORDER
```

References determine dependencies.

File names simply help humans organise the architecture.

---

# 📦 1️⃣5️⃣ Variables — Inputs to Infrastructure

Instead of repeating:

```text
UK South
```

everywhere:

```hcl
variable "location" {
  description = "Azure region used by Cooked"
  type        = string
  default     = "UK South"
}
```

Then:

```hcl
resource "azurerm_resource_group" "cooked" {
  name     = "cooked-dev-rg"
  location = var.location
}
```

Variables are inputs.

```text
INPUT VALUES
↓
TERRAFORM CONFIGURATION
↓
DESIRED INFRASTRUCTURE
```

---

# 🧮 1️⃣6️⃣ Locals — Calculate Once, Reuse

```hcl
variable "environment" {
  type    = string
  default = "dev"
}

locals {
  name_prefix = "cooked-${var.environment}"
}
```

Then:

```hcl
name = "${local.name_prefix}-rg"
```

A local is a named value calculated inside the Terraform configuration.

---

# 📤 1️⃣7️⃣ Outputs — Surface Useful Results

Example:

```hcl
output "resource_group_name" {
  value = azurerm_resource_group.cooked.name
}
```

Outputs mean:

```text
After Terraform understands the infrastructure,
show me this useful value.
```

---

# 🧠 1️⃣8️⃣ Terraform State — Terraform's Memory

Terraform needs to remember which real Azure resources correspond to which Terraform resources.

That memory is called:

```text
STATE
```

Conceptually:

```text
CONFIGURATION

azurerm_resource_group.cooked
↓
STATE

this exact Azure Resource Group
```

State allows Terraform to compare:

```text
what I previously managed
```

with:

```text
what you want now
```

So again:

> **State is memory with rules.**

---

# 🔐 1️⃣9️⃣ State and Secrets

Terraform state can contain important and sometimes sensitive infrastructure information.

Do not casually commit it to Git.

A `.gitignore` might include:

```gitignore
.terraform/
*.tfstate
*.tfstate.*
```

Also:

> **Never submit real passwords, API keys, client secrets or production credentials.**

A real team would deliberately secure shared Terraform state using an appropriate remote backend and access controls.

---

# 🔐 2️⃣0️⃣ Never Hard-Code Secrets

Bad:

```hcl
administrator_password = "Password123!"
```

That secret can now travel through:

```text
Git
pull requests
developer laptops
screenshots
chat
```

Better architecture separates:

```text
CONFIGURATION
```

from:

```text
SECRET
```

Relevant Azure concepts include:

```text
Key Vault
managed identity
secure deployment-time inputs
```

---

# 🏗️ 2️⃣1️⃣ Build a Small Cooked Terraform Skeleton

Start with:

```text
cooked-infrastructure/
│
├── providers.tf
├── variables.tf
├── main.tf
├── outputs.tf
└── terraform.tfvars.example
```

We only need enough infrastructure to understand the pattern before tackling the assessment.

---

# 🔌 2️⃣2️⃣ `providers.tf`

```hcl
terraform {
  required_version = ">= 1.6.0"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

---

# 🎛️ 2️⃣3️⃣ `variables.tf`

```hcl
variable "environment" {
  description = "Environment name"
  type        = string
  default     = "dev"
}

variable "location" {
  description = "Azure region"
  type        = string
  default     = "UK South"
}
```

---

# 🏠 2️⃣4️⃣ Create the Resource Group

`main.tf`

```hcl
resource "azurerm_resource_group" "cooked" {
  name     = "cooked-${var.environment}-rg"
  location = var.location

  tags = {
    application = "cooked"
    environment = var.environment
  }
}
```

Read it as architecture:

```text
Cooked development resources
↓
have a management boundary
↓
Resource Group
```

---

# 🖼️ 2️⃣5️⃣ Add Storage for Fridge Images

Cooked accepts fridge images.

Instead of:

```text
IMAGE BINARY
↓
RELATIONAL DATABASE
```

a more natural model is:

```text
IMAGE BINARY
↓
OBJECT STORAGE

IMAGE REFERENCE / METADATA
↓
DATABASE
```

Terraform:

```hcl
resource "azurerm_storage_account" "cooked" {
  name                     = "cookeddevimages"
  resource_group_name      = azurerm_resource_group.cooked.name
  location                 = azurerm_resource_group.cooked.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  tags = {
    application = "cooked"
    environment = var.environment
  }
}

resource "azurerm_storage_container" "fridge_images" {
  name                  = "fridge-images"
  storage_account_id    = azurerm_storage_account.cooked.id
  container_access_type = "private"
}
```

Dependency graph:

```text
RESOURCE GROUP
↓
STORAGE ACCOUNT
↓
BLOB CONTAINER
```

---

# 🌐 2️⃣6️⃣ Add a Virtual Network

Now ask:

> **Who may communicate with whom?**

```hcl
resource "azurerm_virtual_network" "cooked" {
  name                = "cooked-${var.environment}-vnet"
  address_space       = ["10.20.0.0/16"]
  location            = azurerm_resource_group.cooked.location
  resource_group_name = azurerm_resource_group.cooked.name
}

resource "azurerm_subnet" "backend" {
  name                 = "backend-subnet"
  resource_group_name  = azurerm_resource_group.cooked.name
  virtual_network_name = azurerm_virtual_network.cooked.name
  address_prefixes     = ["10.20.1.0/24"]
}
```

Conceptually:

```text
VNET
=
network boundary

SUBNET
=
smaller communication zone
```

Do not obsess over the CIDR numbers.

Understand the structural purpose:

> **We are creating topology so that communication can be controlled.**

---

# 🔐 2️⃣7️⃣ Add Key Vault

Cooked may need:

```text
database credentials
AI API credentials
other application secrets
```

Represent a Key Vault:

```hcl
data "azurerm_client_config" "current" {}

resource "azurerm_key_vault" "cooked" {
  name                = "cooked-${var.environment}-kv"
  location            = azurerm_resource_group.cooked.location
  resource_group_name = azurerm_resource_group.cooked.name
  tenant_id           = data.azurerm_client_config.current.tenant_id
  sku_name            = "standard"
}
```

Something new appears:

```hcl
data "azurerm_client_config" "current"
```

A **data source** reads information that already exists.

Simple distinction:

```text
resource
↓
manage / create something

data
↓
read something
```

---

# 🗄️ 2️⃣8️⃣ Add PostgreSQL Conceptually

Cooked has persistent structured state:

```text
users
fridges
confirmed ingredients
recipes
dietary preferences
saved recipes
cooking progress
```

That maps naturally to PostgreSQL.

A simplified starting point:

```hcl
resource "azurerm_postgresql_flexible_server" "cooked" {
  name                = "cooked-${var.environment}-postgres"
  resource_group_name = azurerm_resource_group.cooked.name
  location            = azurerm_resource_group.cooked.location
  version             = "16"

  administrator_login    = var.postgres_admin_username
  administrator_password = var.postgres_admin_password

  storage_mb = 32768
  sku_name   = "B_Standard_B1ms"
}
```

The exact production design would need additional networking, resilience, security and cost decisions.

The important mapping is:

```text
PERSISTENT RELATIONAL STATE
↓
POSTGRESQL
↓
AZURE DATABASE FOR POSTGRESQL
↓
TERRAFORM RESOURCE
```

---

# 🔒 2️⃣9️⃣ Sensitive Variables

Example:

```hcl
variable "postgres_admin_username" {
  type    = string
  default = "cookedadmin"
}

variable "postgres_admin_password" {
  type      = string
  sensitive = true
}
```

But remember:

> **`sensitive = true` is not a complete secrets-management strategy.**

Never submit a real password.

---

# 👁️ 3️⃣0️⃣ Add Observability

Cooked must answer:

```text
Is the backend failing?
Are requests slow?
Did AI processing fail?
Can the backend reach the database?
```

That leads to telemetry and logs.

```hcl
resource "azurerm_log_analytics_workspace" "cooked" {
  name                = "cooked-${var.environment}-logs"
  location            = azurerm_resource_group.cooked.location
  resource_group_name = azurerm_resource_group.cooked.name
  sku                 = "PerGB2018"
  retention_in_days   = 30
}

resource "azurerm_application_insights" "cooked" {
  name                = "cooked-${var.environment}-insights"
  location            = azurerm_resource_group.cooked.location
  resource_group_name = azurerm_resource_group.cooked.name
  workspace_id        = azurerm_log_analytics_workspace.cooked.id
  application_type    = "web"
}
```

Again:

```text
INVARIANT
Important failures must be observable
↓
CAPABILITY
Telemetry + logs
↓
AZURE
Monitor / Application Insights
↓
TERRAFORM
Resources describing them
```

---

# ⚙️ 3️⃣1️⃣ Where Should the Backend Run?

Terraform does not make the architecture decision for us.

Possible Azure choices include:

```text
App Service
Container Apps
AKS
Virtual Machines
```

Ask:

```text
How complex is the workload?
Are containers already useful?
Do we actually need Kubernetes?
What operational burden is justified?
```

A more complicated answer is not automatically a better answer.

> **Choose the simplest service that satisfies the real requirement and defend the choice.**

---

# 🖥️ 3️⃣2️⃣ Where Should React Run?

Possible choices might include:

```text
Azure Static Web Apps
App Service
another suitable static-hosting approach
```

Again:

```text
REQUIREMENT
↓
CAPABILITY
↓
SERVICE
```

Do not start with the service name.

---

# ⚡ 3️⃣3️⃣ Where Might Azure Functions Fit?

Cooked contains an image/AI-processing seam:

```text
FRIDGE IMAGE
↓
PROCESSING
↓
AI RECOGNITION
↓
STRUCTURED INGREDIENT RESULT
```

A serverless function can be useful for a bounded event-driven or asynchronous processing task.

But:

> **Using Functions does not mean turning the whole application into microservices.**

Java remains the primary application backend.

---

# 🧠 3️⃣4️⃣ Valid Terraform Does Not Mean Good Architecture

Terraform can describe a terrible architecture perfectly.

For example:

```text
database publicly exposed
hard-coded secrets
everyone has Owner
no monitoring
unnecessary AKS cluster
```

The syntax may validate.

The design can still be poor.

Therefore:

```text
VALID TERRAFORM
≠
GOOD ARCHITECTURE
```

This assessment tests both.

---

# 🌳 3️⃣5️⃣ The Reasoning Algorithm

Before writing a resource, ask:

```text
1. WHAT must remain true?

2. WHAT capability does that require?

3. WHICH Azure service provides that capability?

4. WHAT boundary should contain it?

5. WHO or WHAT may access it?

6. WHICH resources must it communicate with?

7. HOW do we represent those relationships in Terraform?

8. WHAT would Terraform plan to create or change?
```

That is the whole assessment in miniature.

---

---

# 🧪 ASSESSMENT — Cooked Terraform Starter Challenge

## 🎯 The Goal

Your job is to create a **simple Terraform plan for Cooked**.

You are **not** expected to build a production-ready Azure environment.

You are **not** expected to know every Azure service.

You are **not** expected to deploy anything.

The goal is simply to show that you understand this journey:

```text
COOKED NEEDS SOMETHING
↓
choose an Azure resource
↓
describe it in Terraform
↓
connect it to the other resources
↓
explain what Terraform would create
```

Think of this as your first infrastructure design exercise.

---

# 🧩 What You Are Designing

For this assessment, imagine Cooked needs only these core things:

```text
Cooked
│
├── somewhere for the application resources to live
│
├── somewhere to store fridge images
│
├── somewhere to store relational data
│
└── somewhere to keep secrets
```

We can map those needs to Azure:

```text
Application resources
↓
Resource Group

Fridge images
↓
Storage Account + Blob Container

Relational data
↓
Azure Database for PostgreSQL

Secrets
↓
Key Vault
```

That is enough for the core assessment.

You may add more later, but you do not need to.

---

# 📦 What to Submit

Create a folder like:

```text
cooked-infrastructure/
│
├── providers.tf
├── variables.tf
├── main.tf
├── outputs.tf
└── README.md
```

Keep it simple.

The important thing is that another developer can read it and understand what you intended.

---

# 🚦 Step 1 — Start With the Provider

Create `providers.tf`.

Use the AzureRM provider.

A starting point:

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

You should be able to explain:

> **Why does Terraform need a provider?**

You do not need a long answer.

One or two sentences is enough.

---

# 🏠 Step 2 — Create the Cooked Resource Group

In `main.tf`, create one Resource Group.

Example shape:

```hcl
resource "azurerm_resource_group" "cooked" {
  name     = "cooked-dev-rg"
  location = var.location
}
```

Do not blindly copy it.

Make sure you understand:

```text
azurerm_resource_group
=
the type of Azure resource

cooked
=
Terraform's local name

cooked-dev-rg
=
the name Azure will receive
```

---

# 🎛️ Step 3 — Add One Variable

In `variables.tf`, create a variable for the Azure region.

For example:

```hcl
variable "location" {
  type    = string
  default = "UK South"
}
```

Then use:

```hcl
var.location
```

inside your Resource Group.

The important idea is:

```text
VALUE THAT MAY CHANGE
↓
VARIABLE
↓
REUSED BY RESOURCES
```

---

# 🖼️ Step 4 — Add Storage for Fridge Images

Cooked needs somewhere to store uploaded fridge images.

Create:

```text
Storage Account
↓
Blob Container
```

Your Terraform should make the relationship between them clear.

A useful structure is:

```hcl
resource "azurerm_storage_account" "cooked" {
  name                     = "cookeddevimages"
  resource_group_name      = azurerm_resource_group.cooked.name
  location                 = azurerm_resource_group.cooked.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "fridge_images" {
  name                  = "fridge-images"
  storage_account_id    = azurerm_storage_account.cooked.id
  container_access_type = "private"
}
```

Look at the references:

```text
Storage Account
references
Resource Group

Blob Container
references
Storage Account
```

This is how Terraform begins to understand the infrastructure as a dependency graph.

---

# 🔐 Step 5 — Add a Key Vault

Cooked should not keep secrets directly inside the application source code.

Create one Key Vault.

A simplified example:

```hcl
data "azurerm_client_config" "current" {}

resource "azurerm_key_vault" "cooked" {
  name                = "cooked-dev-kv"
  location            = azurerm_resource_group.cooked.location
  resource_group_name = azurerm_resource_group.cooked.name
  tenant_id           = data.azurerm_client_config.current.tenant_id
  sku_name            = "standard"
}
```

You do **not** need to create real secrets.

You only need to understand the purpose:

```text
SECRET
↓
should not live in source code
↓
Key Vault provides a protected place for it
```

---

# 🗄️ Step 6 — Represent PostgreSQL

Cooked needs relational data storage.

Create or sketch the Terraform for:

```text
Azure Database for PostgreSQL
```

You may use the tutorial example as a starting point.

Do not use a real password.

If authentication details make the resource difficult to validate in your environment, it is acceptable to:

```text
write the resource
+
use placeholder variables
+
explain what the variables represent
```

The goal is to understand the mapping:

```text
Cooked needs persistent relational state
↓
PostgreSQL
↓
Azure PostgreSQL resource
↓
Terraform
```

---

# 📤 Step 7 — Add One Output

Create at least one output.

For example:

```hcl
output "resource_group_name" {
  value = azurerm_resource_group.cooked.name
}
```

Be able to explain:

> **What is an output useful for?**

A short explanation is enough.

---

# 🧠 Step 8 — Draw the Dependency Graph

In your README, draw your Terraform architecture.

It can be this simple:

```text
RESOURCE GROUP
│
├── STORAGE ACCOUNT
│   └── BLOB CONTAINER
│
├── KEY VAULT
│
└── POSTGRESQL
```

Then add one or two sentences explaining:

> **Why can Terraform work out that some resources depend on others?**

Hint:

```text
references
```

For example:

```hcl
azurerm_resource_group.cooked.name
```

---

# 🔮 Step 9 — Explain the Expected Plan

You do not need to deploy anything.

Write a short section called:

```text
Expected Terraform Plan
```

Explain what you think Terraform would create.

For example:

```text
I expect Terraform to create:

- one Resource Group
- one Storage Account
- one private Blob Container
- one Key Vault
- one PostgreSQL server
```

Then explain the order conceptually:

```text
Resource Group first
↓
resources that belong inside it
↓
resources that depend on those resources
```

You do not need to predict every line of Terraform's console output.

We care about whether you understand the structure.

---

# ⚙️ Step 10 — Try the Safe Terraform Commands

If Terraform is installed, run:

```bash
terraform fmt
terraform init
terraform validate
```

If you have Azure access and authentication available, you may also try:

```bash
terraform plan
```

But Azure access is **not required** to complete the assessment.

Do not run:

```bash
terraform apply
```

unless specifically told to do so.

---

# ✍️ Final Three Questions

Answer these briefly in your README.

### 1. Why use Terraform instead of creating everything manually in the Azure Portal?

### 2. What is the difference between `terraform plan` and `terraform apply`?

### 3. What does this reference mean?

```hcl
azurerm_resource_group.cooked.name
```

A few sentences for each is enough.

---

# ✅ What a Good Submission Looks Like

A good beginner submission might contain only:

```text
1 Resource Group
1 Storage Account
1 Blob Container
1 Key Vault
1 PostgreSQL resource
1 variable
1 output
1 small architecture diagram
3 short explanations
```

That is completely fine.

The goal is **not complexity**.

The goal is to show that you understand:

```text
WHY a resource exists
↓
HOW Terraform represents it
↓
HOW it relates to other resources
↓
WHAT Terraform would plan to create
```

---

# 🌱 Optional Stretch — Only If You Finish Early

If the core assessment is working and you want to go further, choose **one** of these:

```text
add a Virtual Network and subnet
```

or:

```text
add Application Insights / Log Analytics
```

or:

```text
add a suitable backend hosting resource
```

or:

```text
describe how managed identity + RBAC
could replace stored application credentials
```

Do not attempt all of them.

Choose one and explain why it improves the architecture.

---

# ⚠️ Things We Are Not Assessing Here

You do not need to build:

```text
AKS
complex networking
private endpoints
PIM
management groups
Azure Policy
Sentinel
Defender
Purview
a complete landing zone
a production-grade database topology
```

Those concepts are important to understand.

But this assessment is about your **first clean descent from application requirements into Terraform**.

---

# 🧭 Beginner Checklist

Before submitting, ask:

```text
Does my Terraform have a provider?

Do I have a Resource Group?

Do I use at least one variable?

Do I store fridge images in Blob Storage?

Have I represented PostgreSQL?

Have I represented a Key Vault?

Do my resources reference one another?

Can I draw the dependency graph?

Can I explain what terraform plan means?

Have I avoided real passwords and secrets?
```

If yes, you have done the core task.

---

# ⚡ Final Compression

For this assessment, remember only this:

```text
COOKED NEED
↓
AZURE RESOURCE
↓
TERRAFORM BLOCK
↓
REFERENCE
↓
DEPENDENCY GRAPH
↓
PLAN
```

You are not being tested on how much Azure you can memorise.

You are being tested on whether you can take a small real application and begin turning its infrastructure needs into an **explicit, understandable Terraform model**. ☁️🏗️🌱
