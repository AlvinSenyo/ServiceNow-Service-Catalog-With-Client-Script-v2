# ServiceNow-Service-Catalog-With-Client-Script-v2
**Product:** ServiceNow Service Catalog  
**Requirement:** Display a recommendation for Adobe Photoshop when a user orders a Developer Laptop (Mac) and selects Adobe Acrobat software.

## 📝 Project Overview
This project optimizes the **MacBook Pro** catalog item by providing a contextual prompt. When a developer selects **Adobe Acrobat**, the system automatically suggests **Adobe Photoshop** via a field message.

---

## 🏛️ System Architecture
The solution is built using **ServiceNow Best Practices** for modern development:
* **Strict Mode:** Script Isolation is enabled (True).
* **Update Set Ready:** Both the Catalog Item and Script are packaged for easy migration.

---

## 📂 File Inventory
* [Technical Manual v2](./Technical_Manual.txt) - Detailed, click-by-click build guide for manual replication.
* [Catalog-With-Client-Script v2](./Catalog-With-Client-Script_v2.xml) - The portable Update Set containing the Catalog Item and Client Script logic.
* [UpdateSet_v2.PNG](./UpdateSet_v2.PNG) - Screenshot evidence of the Captured Customer Updates within the Update Set.
* [Output.PNG](./Output_v2.PNG) - Visual verification of the blue recommendation box appearing on the Service Portal.
* [CatalogClientScript_v2.PNG](./CatalogClientScript_v2.PNG) - Reference capture of the Script field and Header configurations.

| Variable | Internal Name | Role |
| :--- | :--- | :--- |
| **Acrobat** | `acrobat` | Trigger (onChange) |
| **Additional Requirements** | `Additional_software_requirements` | Display Target |

---

## 💡 Key Functions
* **Dynamic Messaging:** Uses `g_form.showFieldMsg()` to provide non-intrusive guidance.
* **Lifecycle Support:** The script is configured to display on the Order Form.

---

## 🚀 Deployment Instructions
This solution is packaged in a **Local Update Set**.
1. **Source Instance:** Set State to **Complete** -> **Export to XML**.
2. **Target Instance:** **Import XML** -> **Preview** -> **Commit**.

---

## 🛠️ The Logic

```javascript
/**
 * @description Triggers Photoshop recommendation.
 * @compliance Isolate Script = True
 * @variable_trigger acrobat
 * @variable_target Additional_software_requirements
 */
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    
    // 1. If the form is loading or the value is cleared, stop the script.
    // This prevents the message from showing up empty or prematurely.
    if (isLoading || newValue === '') {
        return;
    }

    // 2. Logic Check: Since this script triggers on the 'acrobat' variable,
    // we check if it has been selected (e.g., if it's a checkbox, value is 'true').
    // If it's a select box, we check the choice value.
    if (newValue == 'true' || newValue == 'acrobat' || newValue == 'yes') {
        
        // 3. Define the recommendation message per requirement.
        var msg = "Recommendation: Adobe Photoshop as well, as it may also be needed for your work.";
        
        // 4. Show the message under the text field.
        // NOTE: The name here must match your variable: Additional_software_requirements
        g_form.showFieldMsg('Additional_software_requirements', msg, 'info');
        
    } else {
        // 5. If they uncheck/deselect Acrobat, hide the recommendation.
        g_form.hideFieldMsg('Additional_software_requirements');
    }
}
