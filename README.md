# 🧩 Terraform State Commands Lab

In this lab, I learned how to **inspect, query, modify, and manipulate Terraform state** using the `terraform state` command family. I practiced listing resources, showing specific resource attributes, removing state entries, and renaming resources both in configuration and in the state file. I also explored how remote state behaves differently from local state.

---

## 📋 Lab Overview

**Goal:**

* Learn core Terraform state manipulation commands
* Inspect resources stored in state
* Remove resources from state management
* Rename resources safely using `terraform state mv`
* Understand when state files exist locally vs remotely

**Learning Outcomes:**

* Use `terraform state list` to inspect resources
* Use `terraform state show` to inspect attributes
* Understand how IDs and metadata are stored in state
* Remove resources using `terraform state rm`
* Rename resources using `terraform state mv`
* Identify when remote backends prevent creation of local `terraform.tfstate`

---

## 🛠 Step-by-Step Journey

### Step 1: Inspect the Project Anime Configuration

**Directory:**

```
/root/terraform-projects/project-anime
```

Using:

```bash
terraform state list
```

This shows all resource names currently stored in the state.

❓ **Which resource names are NOT in state?**
→ Anything not listed by `terraform state list`.

---

### Step 2: Show Attributes for a Specific Resource

To inspect the attributes for the **classics** resource, the correct command is:

```bash
terraform state show local_file.classics
```

✔️ Displays full resource metadata including ID, filename, content, and path.

---

### Step 3: Retrieve the ID of `local_file.top10`

Command:

```bash
terraform state show local_file.top10
```

Returned:

➡️ **ID = A9617…**

This corresponds to the file path of the generated file.

---

### Step 4: Remove a Resource From Terraform State

Terraform should stop managing:

```
/root/anime/hall-of-fame.txt
```

Steps:

1️⃣ Identify it:

```bash
terraform state list
```

2️⃣ Remove it:

```bash
terraform state rm local_file.hall_of_fame
```

3️⃣ Remove the resource block from `main.tf`.

✔️ The file remains on disk, but Terraform no longer manages it.

---

### Step 5: Inspect the Super Pets Configuration

**Directory:**

```
/root/terraform-projects/super-pets
```

Identified resource type:

➡️ `random_pet`

---

### Step 6: Why Is There No terraform.tfstate File?

Answer:

➡️ **Because this configuration uses remote state.**
Remote backends store state externally, not in the local directory.

---

### Step 7: Show the ID of `random_pet.super_pet_2`

Commands:

```bash
terraform state list
terraform state show random_pet.super_pet_2
```

Found ID:

➡️ **wonder-native-token**

(random_pet generates fun, random names)

---

### Step 8: Rename a Resource in Both Config & State

Rename:

```
super_pet_1 → ultra_pet
```

Steps:

1️⃣ Update the name inside `main.tf`.

2️⃣ Update Terraform state:

```bash
terraform state mv random_pet.super_pet_1 random_pet.ultra_pet
```

3️⃣ Re-run:

```bash
terraform plan
terraform apply
# yes
```

✔️ Changes applied successfully.
Terraform shows 2 resources added + 2 destroyed due to name changes.

---

## ✅ Key Commands Summary

| Task                      | Command                          |
| ------------------------- | -------------------------------- |
| List resources in state   | `terraform state list`           |
| Show resource attributes  | `terraform state show <address>` |
| Remove from state         | `terraform state rm <address>`   |
| Rename resources in state | `terraform state mv <old> <new>` |
| Inspect state             | `terraform show`                 |
| Apply changes             | `terraform apply`                |

---

## 💡 Notes / Tips

* `terraform state` commands **do not modify actual infrastructure**, only metadata Terraform tracks.
* Renaming a resource without updating the state results in Terraform trying to recreate it.
* Removing a resource from state leaves the real infrastructure untouched — useful for drift resolution.
* Remote backends (S3, MinIO, Consul, etc.) store state remotely and lock state automatically.
* Always pair `terraform state rm` with removing the resource block from configuration.

---

## 📌 Lab Summary

| Step                                 | Status | Key Takeaways                 |
| ------------------------------------ | ------ | ----------------------------- |
| Inspect Anime project state          | ✅      | Identified existing resources |
| Show resource attributes             | ✅      | Used `state show`             |
| Retrieve ID of top10                 | ✅      | Found via state inspection    |
| Remove hall_of_fame from state       | ✅      | Resource unmanaged            |
| Identify resource type in super-pets | ✅      | `random_pet`                  |
| Understand missing local state       | ✅      | Uses remote backend           |
| Show ID of super_pet_2               | ✅      | Extracted token-based ID      |
| Rename super_pet_1 → ultra_pet       | ✅      | Updated config + state        |

---

## ✅ References

* [Terraform State Commands](https://developer.hashicorp.com/terraform/cli/commands/state)
* [Terraform Resource Addressing](https://developer.hashicorp.com/terraform/language/state/resources)
* [Renaming Resources Safely](https://developer.hashicorp.com/terraform/language/state/mv)
