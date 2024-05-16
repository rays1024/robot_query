# robot_query

## GPT-4 Omni Autonomously Generated Query Prompt


**Task and Scenario Description:**
A human is playing with a construction toy set to assemble a thing from many parts. They have a manual that provides instructions and images of the parts needed. They will ask a robot arm to bring a part to them by describing the part. The human cannot see what parts are available. There might be ambiguity in the human's description or instruction, so the robot needs to query the human in various ways to get more information and elicit an informative response.

The robot is capable of generating three types of queries to identify the correct part requested by the human:

1. **Verbal Only Query:** 
   - The robot can ask questions verbally without performing any physical actions. These questions can be detailed and are not limited in form.

2. **Physical Action Only Query:** 
   - The robot performs a series of physical actions using a combination of the following essential components:
        
        pick(part) # The robot picks up a specified part.
        move(location) # The robot moves to a specified location.
        place() # The robot places the held part at the current location. 
        rotate(angle) # The robot rotates the held part to a specified angle.
        point(location) # The robot uses a pointer to indicate a specific location.
        
   - The robot can place parts in any of the 5 cells in the display area or pick them up from these cells. Each cell in the display area is indexed from 0 to 4.

3. **Combination of Verbal and Physical Action Query:** 
   - The robot can use both verbal questions and physical actions to gather more information from the human. The physical actions must be performed using the components listed above, and the robot can place or pick up parts in the display area by specifying the cell index.

**Information:**
- The potential parts are provided as images, each associated with a probability and an index starting from 0.
- The robot can refer to these parts by their indices when generating queries.
- The respective probability of each part is available to the robot below.

**Probability:**
Here is the list of each potential toy set part's probability of being the one the user asked for. These probabilities are indexed from 0 and correspond to the images provided.
{str(result_prob)}

**Instructions:**
- The robot will decide which type of query to generate based on the context and the current state of information.
- The goal is to accurately identify the part requested by the human with the least amount of ambiguity.
- Generate exactly one appropriate and specific query with '{query_type}' query type with the task in mind.
- Avoid open_ended queries that are hard to answer quickly or without immediate answers.
- Explain what you see and understand from the input, and how you generate the output.

**Examples:**

1. **Verbal Only Query:**
   - "Does the part you need have holes?"
   - "Is the part used for securing two flat pieces together in the assembly?"

2. **Physical Action Only Query:**
   - **Present a Single Part:**
     - pick(1)
     - move(cell_0)
     - place()
     - point(cell_0)

   - **Present Multiple Parts for Comparison:**
     - pick(2)
     - move(cell_0)
     - place()
     - pick(5)
     - move(cell_1)
     - place()

3. **Combination of Verbal and Physical Action Query:**
   - **Compare Parts and Ask for Details:**
     - pick(3)
     - move(cell_0)
     - place()
     - pick(4)
     - move(cell_1)
     - place()
     - "I have placed two parts in the display area. Can you tell me if either of these matches the part you need?"
     - "What are the distinguishing features of the part you're looking for?"

**Example Output JSON Format:**

{{"verbal": "Does the part you need have holes?", "explanation": "gpt_generated_explanation"}}
{{"physical":"pick(1)\nmove(cell_0)\nplace()\npoint(cell_0)", "explanation": "gpt_generated_explanation"}}
{{"combination":"pick(3)\nmove(cell_0)\nplace()\npick(4)\nmove(cell_1)\nplace()\nI have placed two parts in the display area. Can you tell me if either of these matches the part you need?", "explanation": "gpt_generated_explanation"}}

## GPT-4 Omni Simulation Generated Query - Simulation Response Prompt

You are a language model simulating a human interacting with a construction toy set. Your task is to help identify a specific part requested by a user. The interaction follows these steps:

1. **Initial Question/Description from the User:** The user previously asked for a part with a certain description.
2. **Query to Clarify the Request:** To reduce ambiguity and get a clear answer, you will generate a query to ask the user for more information about the part.
3. **Ground Truth:** This is the actual part that the user is referring to.

You need to provide a response to the query with a random persona that is age and gender-neutral to simulate the variability in real users. Your response should include some degree of error or randomness to mimic the potential inaccuracies of human users.

The robot is capable of generating three types of queries to identify the correct part requested by the human:

1. **Verbal Only Query:** 
- The robot can ask questions verbally without performing any physical actions. These questions can be detailed and are not limited in form.

2. **Physical Action Only Query:** 
- The robot performs a series of physical actions using a combination of the following essential components:
        
        pick(part) # The robot picks up a specified part.
        move(location) # The robot moves to a specified location.
        place() # The robot places the held part at the current location. 
        rotate(angle) # The robot rotates the held part to a specified angle.
        point(location) # The robot uses a pointer to indicate a specific location.
        
- The robot can place parts in any of the 5 cells in the display area or pick them up from these cells. Each cell in the display area is indexed from 0 to 4.

3. **Combination of Verbal and Physical Action Query:** 
- The robot can use both verbal questions and physical actions to gather more information from the human. The physical actions must be performed using the components listed above, and the robot can place or pick up parts in the display area by specifying the cell index.

### Inputs:
- **Initial Question/Description:** {user_input}
- **Query Type:** {query_type}
- **Query:** {query_text}
- **Ground Truth:** The ground truth is provided as an image.

### Instructions:
1. Read the initial question/description provided by the user.
2. Use the query to ask for more details about the part.
3. Generate a response with some variability, keeping in mind the ground truth but not strictly adhering to it. Your response should be realistic and reflect how different users might interpret the query.

### Example Response:
---

**Initial Question/Description:** "I need a piece that connects two rods at a right angle."

**Query Type:** verbal

**Query:** "Can you describe the size and color of the piece you're looking for?"

**Random Persona Response:** "It's a small piece, maybe red or orange? It has an U shape, I think."

Please provide your response as a single line of text based on the inputs and instructions above.

## GPT-4 Omni Simulation Generated Query - Estimate Ground Truth Prompt

You are a language model tasked with estimating the ground truth part requested by a user from a list of images. You will be provided with the following information:

1. **Query Text:** A query generated to clarify the user's request.
2. **Response Text:** The user's response to the query, which includes some variability and potential inaccuracies.
3. **List of Potential Ground Truth Images:** Images of parts that could potentially match the user's request.

Your job is to estimate which image best matches the ground truth part based on the query and response.

### Inputs:
- **Query Text:** {query_text}
- **Response Text:** {simulated_response}
- **Potential Ground Truth Images:** You are provided with a series of images of potential ground truths indexed from 0.

### Instructions:
1. Review the query text and the response text to understand the user's description.
2. Examine the list of potential ground truth images.
3. Estimate which image best matches the ground truth based on the user's description in the response text.
4. Generate the index of the image that best matches the ground truth.

### Example Decision-Making Process:
1. **Query Text:** "Can you describe the size and color of the piece you're looking for?"
2. **Response Text:** "It's a small piece, maybe red or orange? It has an U shape, I think."

**Estimated Ground Truth:** 
6

### Your Task:
Please provide and only provide the index as an integer of the estimated ground truth image based on the inputs provided above.
Do not provide any explanation.
