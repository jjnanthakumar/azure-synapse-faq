# Purview Features FAQ

### **Q**: _Can Purview help on Azure Synapse Notebook end to end lineage?_

**A**: As of now, the answer is no but actually we were working on it. (private preview)

---

### **Q**: _Where does it store scanned data and is there any versioning?_

**A**: It stores within the purview catalog as Data Map.

---

### **Q**: _Can this tool scan all DB's and bring some correlation based on data?_

**A**: As of now, there are only a few connectors available but you can use Atlas API to ingest data.

---

### **Q**: _Is there any approval mechanism Workflow on term or glossary?_

**A**: As of now, we don't have the approval workflow mechanism but we are working on it.

---

### **Q**: _What if meta data information changes from the previous scan?_

**A**: If the metadata information changes significantly, then it will update the meta data information in purview.

---

### **Q**: _Can we do combination search like city = 'NY' and sales >= 10000?_

**A**: As of now, Purview is limited to meta data its not for the data itself but if you want to search for data use synapse.

---

### **Q**: _How Purview detect data lineage if relationship between source and sink is logical?_

**A**: It detects from the data factory and flows from the data factory to purview.

---

### **Q**: _Does it scan the value and show a relationship with other relations in the database?_

**A**: As of now, it shows some of the related datasets in the database if you want to define a relationship use Atlas API.

---

### **Q**: _Does this tool capture meta data information of the DB table?_

**A**: Yes, it captures meta data.

---

### **Q**: _Is there any roadmap planned for Snowflake Connector?_

**A**: Yes, will be available soon. (private preview)

---

### **Q**: _Can we extract Purview Report, if yes what is the format?_

**A**: As of now, you can't extract Purview Report but that's on the roadmap.

---

### **Q**: _Can we assign glossary automatically using scripting?_

**A**: Yes, you can use Atlas API.

---

### **Q**: _Can we use Purview to do migration from on-premise to cloud for lineage?_

**A**: Yes, you can use Atlas API to extract and ingest into purview.

---

### **Q**: _Can we create analysis report in powerbi using purview with specific data chosen?_

**A**: As of now, you can't as it is in the road map.

---