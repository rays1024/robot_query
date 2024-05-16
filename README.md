# robot_query

## GPT-4 Omni Autonomously Generated Query


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
