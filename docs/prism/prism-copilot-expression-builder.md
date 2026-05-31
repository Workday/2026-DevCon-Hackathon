# Prism Copilot Expression Builder

## **Overview**

Prism Copilot Expression Builder is an intelligent, generative AI assistant designed to accelerate the data preparation lifecycle within Workday Prism Analytics.

By leveraging natural language processing, the Prism Copilot Expression Builder enables users to generate complex transformation logic, build sophisticated calculations, significantly reducing the time required to move from raw data to actionable insights.

## **Accessing the Prism Copilot Expression Builder**

**Prism Copilot Expression Builder** provides a dedicated interface within the derived dataset’s **“Edit Transformations”** page, where you can describe calculations in plain English. The Prism Copilot Expression Builder is integrated directly into the **Data Catalog** task.

To access Prism Copilot Expression Builder:

1. Navigate to the **Data Catalog** task.  
2. For a derived data set on which you’d like to add a Calculated Field, click on the ellipsis or related actions menu and select **Edit Transformations.**  
3. Click **Add Field**.  
  
   ![image|512x335](upload://1gMQRFyaqL4tRFKZwgJ6y3BYQRs.png)
 
     
   The Prism Copilot Expression Builder Copilot opens a chat box where you can enter your prompt. After you type a prompt and the correct syntax is generated, click **Apply.**  
  
   ![image|512x445](upload://nI9SsAUnmpf5yTOe0h0PGzGblLU.png)

     
     
   

## **Example Prompts**

**Natural Language Expression Building**  
Transform complex business requirements into valid Prism syntax without manual coding.

* **Text & Date Logic:** Use conversational prompts to handle string manipulations (concatenation, casing, extraction) and date calculations (differences, formatting, fiscal periods).  
* Example: "Extract the first 5 characters of the Cost Center and append the Current Year to the end."

**Advanced Window Functions**  
Simplify the creation of "Set-based" calculations that typically require deep technical expertise.

* **Partitioning & Ordering:** Use prompts to define OVER, PARTITION BY, and ORDER BY logic.  
* Examples:  
* "Calculate the average claim amount per agent, partitioned by the 'Region' field."  
* "Create a running total of 'Sales Amount' ordered by 'Transaction Date'."

**Multi-Shot Refinement**  
Prism Copilot Expression Builder supports iterative feedback. If the first generated expression isn't perfect, you can refine it in the same chat session.

* Prompt: "Calculate the difference between Hire Date and Today."  
* Follow-up: "Now format that result as a number of months."

## **Workflow: Using Prism Copilot Expression Builder**

1. **Open Prism Copilot Expression Builder:** Click the **Add Field** icon within the **Edit Transformation** page of a Derived Data Set.  
2. **Describe your Goal:** Enter your request in the chat bar using natural language.  
3. **Preview Logic:** Prism Copilot Expression Builder generates the suggested Prism expression or description.  
4. **Apply & Validate:** Insert the code into your pipeline and use the Prism data preview to verify the output in real-time.

## **Best Practices**

* **Reference Specific Fields:** For the most accurate results, use the exact names of the fields in your dataset. Example: "Worker\_ID" instead of "the ID").  
  Use the square brackets (“\[“) to bring up the prompt for dataset fields.  
* **Iterate for Complexity:** For highly complex logic, use smaller, iterative prompts to ensure Prism Copilot Expression Builder captures every nuance correctly.