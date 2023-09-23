# Generative AI class

Introduction and Course Objectives

Full Transcript: "Hello. And welcome to Introduction to Generative AI. My name is Dr. Gwendolyn Stripling. And I am the artificial intelligence technical curriculum developer here at Google Cloud. In this course, you learn to define generative AI, explain how generative AI works, describe generative AI model types, and describe generative AI applications."

What is Generative AI?
Full Transcript: "Generative AI is a type of artificial intelligence technology that can produce various types of content, including text, imagery, audio, and synthetic data."

Understanding AI and Machine Learning
Full Transcript: "But what is artificial intelligence? Well, since we are going to explore generative artificial intelligence, let's provide a bit of context. So two very common questions asked are what is artificial intelligence and what is the difference between AI and machine learning. One way to think about it is that AI is a discipline, like physics for example. AI is a branch of computer science that deals with the creation of intelligence agents, which are systems that can reason, and learn, and act autonomously. Essentially, AI has to do with the theory and methods to build machines that think and act like humans."

Types of Machine Learning Models and Graphs
Full Transcript: "In this discipline, we have machine learning, which is a subfield of AI. It is a program or system that trains a model from input data. That trained model can make useful predictions from new or never before seen data drawn from the same one used to train the model. Machine learning gives the computer the ability to learn without explicit programming. Two of the most common classes of machine learning models are unsupervised and supervised ML models. The key difference between the two is that, with supervised models, we have labels. Labeled data is data that comes with a tag like a name, a type, or a number. Unlabeled data is data that comes with no tag.

[supervised learning] (https://youtu.be/G2fqAlgmoPo?t=151)

[un-supervised learning] (https://youtu.be/G2fqAlgmoPo?t=181)

[supervised vs unsupervised] (https://youtu.be/G2fqAlgmoPo?t=206)

This graph is an example of the problem that a supervised model might try to solve. For example, let's say you are the owner of a restaurant. You have historical data of the bill amount and how much different people tipped based on order type and whether it was picked up or delivered. In supervised learning, the model learns from past examples to predict future values, in this case tips. So here the model uses the total bill amount to predict the future tip amount based on whether an order was picked up or delivered. This is an example of the problem that an unsupervised model might try to solve. So here you want to look at tenure and income and then group or cluster employees to see whether someone is on the fast track. Unsupervised problems are all about discovery, about looking at the raw data and seeing if it naturally falls into groups."

Deep Learning and Its Role
Full Transcript: "Now that we've explored the difference between artificial intelligence and machine learning, and supervised and unsupervised learning, let's briefly explore where deep learning fits as a subset of machine learning methods. While machine learning is a broad field that encompasses many different techniques, deep learning is a type of machine learning that uses artificial neural networks, allowing them to process more complex patterns than machine learning."

[deep learning neural network] (https://youtu.be/G2fqAlgmoPo?t=279)

[semi-supervised learning - labeled and unlabeled] (https://youtu.be/G2fqAlgmoPo?t=303)

    -   trained on large amount of labeled data and small amount of unlabeled data

Generative AI in the Context of AI and ML
Full Transcript: "Now we finally get to where generative AI fits into this AI discipline. Gen AI is a subset of deep learning, which means it uses artificial neural networks, can process both labeled and unlabeled data using supervised, unsupervised, and semi-supervised methods."

Generative vs. Discriminative Models
Full Transcript: "Large language models are also a subset of deep learning. Deep learning models, or machine learning models in general, can be divided into two types, generative and discriminative. A discriminative model is a type of model that is used to classify or predict labels for data points. A generative model generates new data instances based on a learned probability distribution of existing data."

[discriminitive vs generative] (https://youtu.be/G2fqAlgmoPo?t=396)

[example] (https://youtu.be/G2fqAlgmoPo?t=404)

[predictive model vs GenAI] (https://youtu.be/G2fqAlgmoPo?t=453)

[what is and is not genai] (https://youtu.be/G2fqAlgmoPo?t=467)

[y=f(x)] (https://youtu.be/G2fqAlgmoPo?t=491)

[summary at high level - discriminitive] (https://youtu.be/G2fqAlgmoPo?t=544)

[summary at high level - generative] (https://youtu.be/G2fqAlgmoPo?t=567)

[GenAI formal definition] (https://youtu.be/G2fqAlgmoPo?t=655)

[language and image models] (https://youtu.be/G2fqAlgmoPo?t=686)

[gen image model] (https://youtu.be/G2fqAlgmoPo?t=729)

[gen language model] (https://youtu.be/G2fqAlgmoPo?t=742)

[predict what comes next] (https://youtu.be/G2fqAlgmoPo?t=764)

[transformers] (https://youtu.be/G2fqAlgmoPo?t=822)

[hallucinations] (https://youtu.be/G2fqAlgmoPo?t=847)

[hallucinations-causes] (https://youtu.be/G2fqAlgmoPo?t=863)

[prompt-design] (https://youtu.be/G2fqAlgmoPo?t=888)

Types of Generative AI Models
Full Transcript: "So what are some examples of generative AI models? Well, there are many types of generative AI models. And they can be categorized by the type of input and output they use. Text-to-text models take a natural language input and produce a text output. 

[text-to-text] (https://youtu.be/G2fqAlgmoPo?t=939)

Text-to-image models are trained on a large set of images, each captioned with a short text description. 

[text-to-image] (https://youtu.be/G2fqAlgmoPo?t=957)

    -   can use diffusion

Text-to-video models aim to generate a video representation from text input.

[Text-to-video] (https://youtu.be/G2fqAlgmoPo?t=966)


 Text-to-3D models generate three-dimensional objects that correspond to a user's text description. 
 
 [Text-to-task] (https://youtu.be/G2fqAlgmoPo?t=999) models are trained to perform a defined task or action based on text input."

[foundation-model](https://youtu.be/G2fqAlgmoPo?t=1028)

[vertex-ai-foundation-models] (https://youtu.be/G2fqAlgmoPo?t=1070)

Applications and Tools for Generative AI
Full Transcript: "So what are some examples of generative AI applications? Generative AI applications include code generation, sentiment analysis, occupancy analytics, etc. [Generative AI Studio](https://youtu.be/G2fqAlgmoPo?t=1186) lets you quickly explore and customize gen AI models. [Generative AI App Builder](https://youtu.be/G2fqAlgmoPo?t=1225) lets you create gen AI apps without having to write any code."

Conclusion and Additional Resources
Full Transcript: "Generative AI Studio is a web-based tool that helps users to interact with the app using natural language. You can create your own digital assistants, custom search engines, knowledge bases, training applications, and much more. [PaLM API](https://youtu.be/G2fqAlgmoPo?t=1270) lets you test and experiment with Google's large language models and gen AI tools. To make prototyping quick and more accessible, developers can integrate PaLM API with Maker suite and use it to access the API using a graphical user interface. The suite includes a number of different tools such as a model training tool, a model deployment tool, and a model monitoring tool. The model training tool helps developers train ML models on their data using different algorithms. The model deployment tool helps developers deploy ML models to production with a number of different deployment options. The model monitoring tool helps developers monitor the performance of their ML models in production using a dashboard and a number of different metrics

***

