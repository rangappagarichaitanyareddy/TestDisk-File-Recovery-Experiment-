# Experiment 2: Recover Deleted or Damaged Files from a Storage Device Using TestDisk

---

## Aim

To recover deleted or missing partitions and repair a corrupted partition or damaged NTFS boot sector from a storage device using **TestDisk**.

---

## Software Used

* **TestDisk** (Data Recovery Utility)
* **Linux Operating System** (Kali Linux / Ubuntu)

---

## Introduction

**TestDisk** is a powerful open-source data recovery and disk repair utility designed to recover lost partitions and make non-booting disks bootable again when these symptoms are caused by faulty software, certain types of viruses, or human error (such as accidentally deleting a partition table).

### Key Capabilities of TestDisk:
* **Recover deleted or lost partitions**
* **Rebuild and recover damaged partition tables**
* **Repair corrupted file systems (NTFS, FAT32, ext2/ext3/ext4)**
* **Recover deleted files from file systems**
* **Repair NTFS boot sectors using backup boot sectors**
* **Recover partitions from damaged or RAW storage devices**
* **Restore partition structures and bootable flags**

In this experiment, TestDisk was utilized on a **Linux operating system** to analyze a storage device, perform quick and deeper searches for missing partitions, inspect partition file structures, change partition flags to restore logical partitions, and repair a damaged NTFS boot sector from a valid backup.

---

## Procedure

### Step 1: Open TestDisk

1. Open the **Terminal** in the Linux operating system.
2. Launch the TestDisk utility with administrative privileges:
   ```bash
   sudo testdisk /dev/loop0
   ```
3. TestDisk displays the log creation interface with three available options:
   * **Create** – Creates a new log file (`testdisk.log`) containing technical information and messages.
   * **Append** – Appends recovery messages and technical data to an existing log file.
   * **No Log** – Runs TestDisk session without recording technical logs.
4. Select **Create** and press **Enter** to proceed.

---

### Step 2: Select Disk

1. TestDisk scans and displays all detected storage devices along with their total capacity, sector sizes, and identifiers.
2. Use the **Up/Down Arrow keys** to select the target storage device containing the missing or damaged partition (e.g., `/dev/loop0`).
3. Select **[Proceed]** and press **Enter**.

---

### Step 3: Select Partition Table Type

1. TestDisk displays available partition table architecture types:
   * **Intel** – Intel/PC partition (MBR)
   * **EFI GPT** – GUID Partition Table (GPT)
   * **Humax** – Humax partition table
   * **Mac** – Apple partition map
   * **None** – Non-partitioned media
   * **Sun** – Sun Solaris partition
   * **XBox** – Xbox partition
2. TestDisk automatically detects and highlights the default partition table type (**Intel**).
3. Select **Intel** and press **Enter**.

---

### Step 4: Analyse the Partition Structure

1. From the TestDisk main menu, select **Analyse** to examine the current partition structure and detect lost partitions.
2. Press **Enter** to start the analysis.
3. The current partition structure is inspected for:
   * Missing partitions
   * Duplicate partition entries
   * Invalid partition entries and missing endmarks (`0xAA55`)
   * File-system inconsistencies
   * Corrupted partition boot sectors
4. The analysis identified an invalid partition entry and reported **Invalid NTFS boot**, indicating corruption in the primary boot sector.

---

### Step 5: Start Quick Search

1. Select **Quick Search** from the bottom menu and press **Enter**.
2. TestDisk performs a rapid scan across disk cylinders and sectors to locate recognized partition signatures.
3. The search detects available partitions in real-time, including the missing logical partition.

---

### Step 6: Verify the Recovered Partition

1. Highlight the detected partition from the list.
2. Press **P** to list the files and directory structure stored inside the partition.
3. Verify that the expected user files, directories, and documents are visible.
4. Use arrow keys to browse files and directories.
5. Press **Q** to exit the file listing and return to the partition menu.

---

### Step 7: Perform Deeper Search

1. If the required partition is not fully recovered or additional partitions are missing, select **Deeper Search** and press **Enter**.
2. Deeper Search conducts a comprehensive sector-by-sector scan across the entire storage device.
3. It actively searches for:
   * FAT32 backup boot sectors
   * NTFS backup boot sectors and MFT mirrors
   * ext2/ext3/ext4 backup superblocks
   * Additional lost or deleted partitions

---

### Step 8: Examine Deeper Search Results

1. Upon completion of the Deeper Search, TestDisk displays all identified partition headers.
2. TestDisk detects the missing partition using its backup boot sector:
   ```text
   NTFS found using backup sector!
   ```
3. Overlapping or deleted partitions are initially displayed with the status **D (Deleted)**.

---

### Step 9: Verify the Damaged Partition

1. Highlight the first detected partition entry and press **P** to list files.
2. The file system for this entry is damaged or unreadable, and files are not accessible.
3. Press **Q** to return to the partition list.
4. Leave this invalid/corrupted entry marked as **D (Deleted)** so it is excluded from recovery.

---

### Step 10: Identify the Correct Partition

1. Highlight the second partition entry with the corresponding label.
2. Press **P** to list files.
3. The files and directory structures are correctly listed and verified intact.
4. Press **Q** to return to the partition list.

---

### Step 11: Change Partition Status

1. TestDisk uses specific status flags to define partition types:
   * `*` – Primary bootable
   * `P` – Primary
   * `L` – Logical
   * `D` – Deleted
2. Highlight the valid partition previously marked as **D (Deleted)**.
3. Use the **Left/Right Arrow keys** to switch the status:
   $$\text{D (Deleted)} \longrightarrow \text{L (Logical)}$$
4. Press **Enter** to proceed to the write menu.

---

### Step 12: Write the Recovered Partition Table

1. From the confirmation menu, select **Write** and press **Enter**.
2. TestDisk prompts: `Write partition table, confirm ? (Y/N)`
3. Press **Y** to confirm writing the updated partition table structure to disk.
4. The recovered partition structure is permanently saved to the partition table.

---

### Step 13: Check the NTFS Boot Sector

1. TestDisk evaluates the status of the NTFS boot sector for the recovered partition.
2. The primary boot sector displays status **Bad**, while the backup boot sector is verified as **Valid / OK**.
3. The valid backup boot sector is selected to repair the primary boot sector.

---

### Step 14: Repair the NTFS Boot Sector

1. Select the **Backup BS** (Backup Boot Sector) option.
2. Press **Enter** to proceed.
3. Press **Y** when prompted to overwrite the corrupted primary boot sector with the valid backup boot sector.
4. TestDisk copies the backup boot sector to the main NTFS boot sector location.

---

### Step 15: Verify Boot Sector Recovery

1. After copying, TestDisk reports that both the primary boot sector and backup boot sector are **identical and OK**:
   ```text
   Boot sector: OK
   Backup boot sector: OK
   Sectors are identical.
   ```
2. Press **Enter** to return to the main menu.

---

### Step 16: Restart the Linux System

1. TestDisk displays the final message:
   ```text
   You have to restart your Computer to access your data.
   ```
2. Quit TestDisk safely and restart the Linux system.
3. Upon system reboot, mount the recovered storage device and verify complete read/write access to all restored files and folders.

---

## Recovery Verification

| Parameter          | Result                       |
| ------------------ | ---------------------------- |
| Recovery Tool      | TestDisk                     |
| Operating System   | Linux                        |
| Operation          | Lost Partition Recovery      |
| Search Method      | Quick Search / Deeper Search |
| Partition Status   | D (Deleted) → L (Logical)    |
| File Listing       | Files successfully displayed |
| Partition Table    | Successfully recovered       |
| NTFS Boot Sector   | Repaired                     |
| Backup Boot Sector | Valid                        |
| Boot Sector Status | OK                           |
| Data Recovery      | Successful                   |

---

## Result

The missing/damaged partition was successfully identified and recovered using **TestDisk** on a **Linux operating system**.

The correct partition was verified by listing its files, and its status was changed from **D (Deleted)** to **L (Logical)**. The recovered partition structure was successfully written to the partition table.

The damaged NTFS boot sector was also repaired using the valid backup boot sector.

---

# Evidence / Screenshots

### Screenshot 1 – TestDisk Disk Selection

![TestDisk Disk Selection](https://github.com/user-attachments/assets/6f5c360f-3e5f-47d9-8920-49ea7f2827ae)

---

### Screenshot 2 – Proceed / Disk Selection

![TestDisk Proceed](https://github.com/user-attachments/assets/ba10881c-a93e-4711-8073-0b815c418921)

---

### Screenshot 3 – Partition Table Selection

![Partition Table Selection](https://github.com/user-attachments/assets/8b0e212a-ea9b-4303-8659-8fe68842251a)

---

### Screenshot 4 – Partition Analysis

![Partition Analysis](https://github.com/user-attachments/assets/559533d1-73ea-42ad-9816-369d70d930e8)

---

### Screenshot 5 – TestDisk Recovery Screen

![TestDisk Recovery](https://github.com/user-attachments/assets/971db9dd-7e2a-4f27-a6ad-0c4eeef3769c)

---

### Screenshot 6 – Partition Structure / Recovery Result

![Recovery Result](https://github.com/user-attachments/assets/13b5de53-1077-4d28-8117-d9f90c770bde)
