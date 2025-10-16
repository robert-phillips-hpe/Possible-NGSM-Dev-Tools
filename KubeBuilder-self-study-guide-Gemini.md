# KubeBuilder Operator Development Self-Study Guide (The Go Control Loop)

As a technical instructor with 20 years in the industry, I've designed this intensive self-study curriculum to transform you from a Kubernetes user to a **Kubernetes API extender**—a skill that’s becoming crucial in modern cloud-native architecture. This course is for those who are ready to dive deep into the **Operator Pattern** using **KubeBuilder**.

The entire curriculum is estimated to take **24-32 hours** to complete, depending on your familiarity with Go and the Kubernetes API.

---

## Course Outline

| Module | Topic | Estimated Time |
| :--- | :--- | :--- |
| **01** | Kubernetes & Operator Fundamentals | 3 Hours |
| **02** | KubeBuilder Quick Start & Project Structure | 4 Hours |
| **03** | Custom Resource Definition (CRD) Development | 5 Hours |
| **04** | The Controller and Reconciliation Loop (Controller-Runtime) | 8 Hours |
| **05** | Advanced Topics: Webhooks, Tests, and Finalizers | 4 Hours |
| **06** | Final Project: Building a Production-Ready Operator | 4 Hours |
| **Total** | | **28 Hours** |

---

## Prerequisites and Required Skills

To successfully complete this self-study guide, you need more than just curiosity; you need a solid technical foundation. The items below are non-negotiable.

### **Required Knowledge & Skills**

* **Go Programming Language (Mandatory)**
    * **Explanation:** KubeBuilder generates code in Go, and the entire controller logic is written using its libraries. You must be comfortable with Go syntax, structs, interfaces, pointers, and concurrency (goroutines).
    * **Source: Go Tour (Official):** `https://go.dev/tour/`
    * **Source: Go by Example (Non-Official):** `https://gobyexample.com/`

* **Core Kubernetes Concepts**
    * **Explanation:** You must have a deep understanding of standard Kubernetes resources (Pods, Deployments, Services) and the concept of **desired state** versus **actual state**. Crucially, you need to understand the **Controller Pattern** and the basic **API architecture**.
    * **Source: Kubernetes Controllers (Official):** `https://kubernetes.io/docs/concepts/architecture/controller/`
    * **Source: The Operator Pattern Explained (Official Kubernetes):** `https://kubernetes.io/docs/concepts/extend-kubernetes/operator/`

* **YAML/JSON Proficiency**
    * **Explanation:** Kubernetes resources are defined in YAML. Operator development involves reading, writing, and understanding complex YAML manifests.
    * **Source: YAML Tutorial (Non-Official):** `https://yaml.org/`

### **Technical Prerequisites (Installation)**

* **Go Environment**
    * **Requirement:** Go version `v1.20.0+`
    * **Source: Official Go Download:** `https://go.dev/dl/`

* **Docker**
    * **Requirement:** Docker version `17.03+` to build and push the operator image.
    * **Source: Official Docker Install Guide:** `https://docs.docker.com/get-docker/`

* **Kubectl**
    * **Requirement:** `kubectl` for interacting with the cluster.
    * **Source: Official Kubectl Install Guide:** `https://kubernetes.io/docs/tasks/tools/install-kubectl/`

* **Kubernetes Cluster Access**
    * **Requirement:** Access to a working Kubernetes cluster. **Kind** is recommended for local development.
    * **Source: Kind Documentation (Non-Official):** `https://kind.sigs.k8s.io/docs/user/quick-start/`

---

## Module 01: Kubernetes & Operator Fundamentals

**Time Frame:** 3 Hours

**Explanation:** Establish the theoretical context. What is an **Operator**, and why do we need one? Focus on the **declarative model** and the role of the **Custom Resource Definition (CRD)** in extending the Kubernetes API.

### **Online Resources**

* **Kubernetes Documentation on Custom Resources:** `https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/`
* **The Operator Pattern (Official Kubernetes):** `https://kubernetes.io/docs/concepts/extend-kubernetes/operator/`
* **Article on CRDs, Controllers, and Operators:** `https://www.vcluster.com/blog/extending-kubernetes-with-custom-resource-definitions-crds`

### **Exercises**

1.  **Read & Summarize:** Read the resources above and write a two-paragraph summary explaining the difference between a **Deployment** and an **Operator-managed Custom Resource**.
2.  **Manifest Analysis:** Find an open-source Operator's CRD YAML manifest and identify the **group**, **version**, **scope**, and **names**.

### **Questions to Test Understanding**

1.  What core Kubernetes design principle does the Operator Pattern embody?
2.  What is the function of a **CRD** in a Kubernetes cluster?
3.  Why is an Operator necessary for stateful applications, but not strictly required for stateless ones?

---

## Module 02: KubeBuilder Quick Start & Project Structure

**Time Frame:** 4 Hours

**Explanation:** Your first interaction with the KubeBuilder tool. Install it, initialize a new project, and dissect the boilerplate code, focusing on the **Makefile**, `cmd/main.go`, and the **PROJECT** file.

### **Online Resources**

* **KubeBuilder Quick Start (Official):** `https://book-v3.book.kubebuilder.io/quick-start.html`
* **KubeBuilder Project Layout:** `https://book-v3.book.kubebuilder.io/basics/project_layout.html`
* **Blog: Kubebuilder: Easily create a Kubernetes operator:** `https://enix.io/en/blog/kubebuilder/`

### **Exercises**

1.  **Setup and Init:** Install KubeBuilder. Run `kubebuilder init`. Successfully build and run the boilerplate controller locally using `make run`.
2.  **Code Scaffolding:** Create a new API using `kubebuilder create api`. Examine the files in `api/` and `internal/controllers/`. What is the purpose of the `zz_generated.deepcopy.go` file?

### **Questions to Test Understanding**

1.  What is the significance of the `--domain` flag during project initialization?
2.  Which file is the *entry point* for the entire operator? (Hint: it initializes the **Manager**).
3.  What command do you use to generate the CRD manifest and **RBAC** rules after modifying your API types?

---

## Module 03: Custom Resource Definition (CRD) Development

**Time Frame:** 5 Hours

**Explanation:** Focus on defining the **Spec** (desired state) and **Status** (observed state) of your custom resource. Learn about **Go Struct Tags** and **`+kubebuilder` markers** used by `controller-gen` to automatically generate your CRD YAML.

### **Online Resources**

* **Defining the API (Official KubeBuilder):** `https://book-v3.book.kubebuilder.io/basics/api-design.html`
* **CRD Validation (Official Kubernetes):** `https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/`
* **A Hands-On Guide to Kubernetes CRDs (Non-Official):** `https://medium.com/@muppedaanvesh/a-hand-on-guide-to-kubernetes-custom-resource-definitions-crds-with-a-practical-example-%EF%B8%8F-84094861e90b`

### **Exercises**

1.  **Define Spec and Status:** In your generated API file, define a `CustomResource` with a **Spec** that includes a string for a `DeploymentName` and an integer for `ReplicaCount`. Define a **Status** field that tracks the `AvailableReplicas`.
2.  **Implement Validation:** Add a `+kubebuilder:validation:Minimum=1` marker to the `ReplicaCount` field. Run the generation commands and verify this constraint appears in the generated CRD YAML.
3.  **Deploy and Test:** Deploy the new CRD to your cluster (`make install`). Try creating a custom resource with `ReplicaCount: 0`. Does the Kubernetes API server reject it?

### **Questions to Test Understanding**

1.  Explain the difference between a **CustomResource** (CR) and a **CustomResourceDefinition** (CRD).
2.  What is the primary role of the **Status** subresource?
3.  How do you ensure a user cannot set a string field in your **Spec** to a value longer than 100 characters?

---

## Module 04: The Controller and Reconciliation Loop

**Time Frame:** 8 Hours

**Explanation:** This is the core of the Operator. You will work inside the **Reconcile** function. This function implements the control loop: **Watch** the custom resource, **Fetch** its current state, **Compare** to the desired state, and **Act** by creating/updating other Kubernetes resources to match the desired state.

### **Online Resources**

* **Implementing the Controller (Official KubeBuilder):** `https://book-v3.book.kubebuilder.io/basics/controller-implementation.html`
* **The Reconcile Loop Explained (Non-Official):** `https://medium.com/@muradu.iurie.1986/controller-runtime-write-your-own-kubernetes-operators-from-scratch-bcd9c7fb81fa`
* **Operator SDK Go Tutorial (Focus on Reconciliation Logic):** `https://sdk.operatorframework.io/docs/building-operators/golang/tutorial/`

### **Exercises**

1.  **Implement Fetch Logic:** In the **Reconcile** function, write the Go code to fetch the custom resource instance. Handle the `NotFound` error.
2.  **Create a Downstream Resource:** Implement the logic to create a standard Kubernetes **Deployment** that mirrors the `ReplicaCount` from your `CustomResource.Spec`. Use the `Client.Get` and `Client.Create` methods.
3.  **Update and OwnerReferences:** Modify the **Reconcile** function to check if the **Deployment** exists and update its `ReplicaCount` if needed. Ensure the **Deployment** has an **OwnerReference** pointing back to the `CustomResource`.

### **Questions to Test Understanding**

1.  What is the purpose of the **ReconcileResult** struct returned by the **Reconcile** function? When would you return a **Requeue**?
2.  What mechanism prevents a child resource (like a **Deployment**) from being orphaned when its parent `CustomResource` is deleted?
3.  How does a controller track events for resources it does *not* own (e.g., a **Secret** it needs to monitor)?

---

## Module 05: Advanced Topics: Webhooks, Tests, and Finalizers

**Time Frame:** 4 Hours

**Explanation:** Covers critical components for robust operators. **Webhooks** are used for pre-flight checks on user-submitted CRs. **Finalizers** are essential for cleaning up external infrastructure *before* Kubernetes deletes the **CustomResource**. **Integration Tests** are written with `envtest`.

### **Online Resources**

* **KubeBuilder Webhooks (Official):** `https://book-v3.book.kubebuilder.io/reference/webhook-scaffolding.html`
* **Writing Tests with envtest (Official KubeBuilder):** `https://book-v3.book.kubebuilder.io/cronjob-tutorial/writing-tests.html`
* **KubeBuilder and Operator-SDK Tips and Tricks (Finalizers):** `https://sklar.rocks/kubebuilder-tips/`

### **Exercises**

1.  **Implement a Finalizer:** Add a finalizer string to your `CustomResource`. Update the **Reconcile** function to check for the deletion timestamp, perform a mock external cleanup, and then remove the finalizer.
2.  **Create a Validating Webhook:** Use `kubebuilder create webhook`. Write logic to reject any `CustomResource` where `ReplicaCount` is an odd number. Test this by applying an invalid CR manifest.

### **Questions to Test Understanding**

1.  What is the difference between a **Validating Webhook** and a **Mutating Webhook**?
2.  Why must an Operator always remove its own **finalizer** *after* its cleanup logic is complete?
3.  What component does `envtest` start for integration testing? Why is this faster than using a full cluster?

---

## Module 06: Final Project: Building a Production-Ready Operator

**Time Frame:** 4 Hours

**Explanation:** Bring it all together. Refine your project, ensure all **Status** fields are correctly reported, build the final Docker image, and deploy it to a cluster for end-to-end testing.

### **Online Resources**

* **Running in Cluster (Official KubeBuilder):** `https://book-v3.book.kubebuilder.io/quick-start.html#run-it-on-the-cluster`
* **Building an Operator with KubeBuilder (Non-Official Example):** `https://www.civo.com/learn/creating-a-kubernetes-operator-with-kubebuilder`

### **Exercises**

1.  **Deployment:** Build and push your operator Docker image (`make docker-build docker-push`). Deploy your controller to the cluster (`make deploy`).
2.  **E2E Testing:** Create, update, and delete instances of your `CustomResource`. Verify that the underlying **Deployments** and **Pods** are created and garbage-collected correctly.
3.  **Status Reporting:** Ensure your controller continuously updates the **Status** of the `CustomResource` to reflect the *actual* available replicas. Check this using `kubectl get <your-cr> -o yaml`.

### **Questions to Test Understanding**

1.  What is the purpose of the **IMG** variable when running `make deploy`?
2.  If the controller's **Deployment** fails to pull the image, which Kubernetes object's status would you check first to debug the issue?
3.  How would you configure the controller to only watch resources in a specific namespace instead of the entire cluster?

---

## Validated URL List

The following URLs were included in the self-study guide and have been confirmed as accessible and functional for self-study purposes:

| URL | Source/Topic |
| :--- | :--- |
| `https://go.dev/tour/` | Go Programming (Official) |
| `https://gobyexample.com/` | Go Programming (Non-Official) |
| `https://kubernetes.io/docs/concepts/architecture/controller/` | Kubernetes Controller Concepts |
| `https://kubernetes.io/docs/concepts/extend-kubernetes/operator/` | The Operator Pattern |
| `https://yaml.org/` | YAML Tutorial |
| `https://go.dev/dl/` | Go Download |
| `https://docs.docker.com/get-docker/` | Docker Install Guide |
| `https://kubernetes.io/docs/tasks/tools/install-kubectl/` | Kubectl Install Guide |
| `https://kind.sigs.k8s.io/docs/user/quick-start/` | Kind (Kubernetes in Docker) |
| `https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/` | Kubernetes Custom Resources |
| `https://www.vcluster.com/blog/extending-kubernetes-with-custom-resource-definitions-crds` | CRD and Operator Article |
| `https://book-v3.book.kubebuilder.io/quick-start.html` | KubeBuilder Quick Start |
| `https://book-v3.book.kubebuilder.io/basics/project_layout.html` | KubeBuilder Project Layout |
| `https://enix.io/en/blog/kubebuilder/` | KubeBuilder Overview Article |
| `https://book-v3.book.kubebuilder.io/basics/api-design.html` | KubeBuilder API Design |
| `https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/` | CRD Validation & Versioning |
| `https://medium.com/@muppedaanvesh/a-hand-on-guide-to-kubernetes-custom-resource-definitions-crds-with-a-practical-example-%EF%B8%8F-84094861e90b` | CRD Hands-On Guide |
| `https://book-v3.book.kubebuilder.io/basics/controller-implementation.html` | KubeBuilder Controller Implementation |
| `https://medium.com/@muradu.iurie.1986/controller-runtime-write-your-own-kubernetes-operators-from-scratch-bcd9c7fb81fa` | Controller-Runtime / Reconcile Loop |
| `https://sdk.operatorframework.io/docs/building-operators/golang/tutorial/` | Operator SDK Go Tutorial (Logic Example) |
| `https://book-v3.book.kubebuilder.io/reference/webhook-scaffolding.html` | KubeBuilder Webhooks |
| `https://book-v3.book.kubebuilder.io/cronjob-tutorial/writing-tests.html` | KubeBuilder Testing with envtest |
| `https://sklar.rocks/kubebuilder-tips/` | Finalizers and Tips Article |
| `https://book-v3.book.kubebuilder.io/quick-start.html#run-it-on-the-cluster` | KubeBuilder Running in Cluster |
| `https://www.civo.com/learn/creating-a-kubernetes-operator-with-kubebuilder` | Final Project Example |
