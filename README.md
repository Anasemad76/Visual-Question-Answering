
# Visual Question Answering 

Team members: AnasEmad:7048, MohabAyman:7127, MazenGhanem:6896

Date: June25,2023

**Abstract**

In this work, we address the challenges associated with training multi-modality tasks, partic-ularlyvisualquestionanswering,whichisknownforitshighcomplexityandresource-intensiverequirements.Toovercomethesechallenges,weproposeanovelarchitectureinspiredbyCLIPthat eliminates the need for fine-tuning feature extractors.Instead, we adopt a simple yet effectiveapproach of applying a linear classifier to the concatenated features of image and text encoders.Furthermore,our training process incorporates an auxiliary loss that focuses on the predictionofanswertypes.Thisclassificationoutcomeservesasanattentiongate,allowingustoselectappropriateanswerclasses.OurexperimentalresultsareontheVizWiz2022VisualQuestionAnsweringDataset.

1.
# Introduction

CLIP (Contrasting Language-Image Pretraining) is an innovative neural network model developed byOpenAI, designed to establish a strong connection between images and text.Trained on a massivedataset of image-text pairs, CLIP exhibits exceptional capabilities in tasks such as image captioning,image retrieval, and zero-shot learning. This powerful tool enables generating descriptive captions forimages, retrieving relevant images based on textual queries, and even classifying images into novelcategories without prior exposure.With its potential to enhance image search engines and createinteractive visual experiences, CLIP opens up exciting possibilities at the intersection of images andtext. In this project report, we delve into the architecture, training process, and applications of CLIP,sheddinglightonitsremarkableabilitytobridgethegapbetweenvisualandtextualdomains.

1.
# Methodology

We were inspired from the paper [Less Is More: Linear Layers on CLIP Features as Powerful VizWiz](https://arxiv.org/pdf/2206.05281v1.pdf)[Model](https://arxiv.org/pdf/2206.05281v1.pdf).

- **Data**** preprocessing**:TheVizWizdatasetwasloadedusingthepandaslibrarytoanalyzeitsstructureandorganization.Followingthestepsoutlinedinthepaper,weproceededtoextractthetrueanswersfromthe"answers"columnbyselectingthemostcommonanswerforeachlabel.In cases where multiple labels had the same number of votes, we employed the Levenshteinalgorithmasatiebreaker.Thisprocessresultedinavocabularysizeof5458.

Tooptimizeprocessingtime,wechosethe **ViT-B/32** model,whichoffersreducedencodedfeaturesizesof512.Theimageswerereadfromtheirrespectivepathsafterstratifiedsplitting.Weappliedpreprocessingtechniquestotheimages,includingencoding,andsavedtheprocesseddatainatorchfilesusingtorchsave.Thisstepallowedustoreducethetimerequiredforreadingthe data during subsequent sessions.Similar procedures were performed on the questions, and theresulting encoded data was concatenated with the images.Overall, these steps ensured that theVizWizdatasetwasappropriatelypreparedforfurtheranalysisandmodeling.ThedistributionofouranswertypesisdisplayedinFigure1

- **Model**** Designing ****and**** Implementation:**

The model's data loader takes encoded image-question, true answer types, true answers, andtrueanswer-abilityasinputs.Themodelconsistsoffourgates.TheMainGateincludesa

normalization layer witha dropout rateof 0.5and afully connectedlayer with512 outputchannels.The Main Gate's output branches into three branches: **Answer Gate, Answer Type**** Gate, ****and**** Answer-ability ****Gate.** TheAnswerGateemploysafullyconnectedlayerwithnormalization and dropout layers. The Answer Type Gate goes through a fully connected layer,followed by projection onto a layer matching the vocabulary size, and then a sigmoid layer. TheAnswer-ability Gate passes through a fully connected layer, sigmoid activation, and softmax todetermineanswer-ability.Thisarchitectureachievesexcellentresultsinvisualquestionansweringtasks.

- **Model Evaluation:** We encoded our true answers using one hot encoding, and mapped theanswertypestoanumericlist,andanswerabilitywasjustinsertedwithnochanges.in **Trial**** 1**lownumberofepochs(30)andnohypertuningandnoregulization

| TrainingAccuracy | TestingAccuracy | ValidationAccuracy |
| --- | --- | --- |
| 0.5177 | 0.2843 | 0.2704 |

Model 1 Accuracy Table

![Shape1](RackMultipart20230804-1-9h769v_html_e762df3acdb1a056.gif)Model1LossTable

In **Trial**** 2** hyper tuning and data-augmentation andregularization with l2 epochs(100)

| TrainingAccuracy | TestingAccuracy | ValidationAccuracy |
| --- | --- | --- |
| 0.7877 | 0.558 | 0.531 |

Model 2 Accuracy Table

![Shape2](RackMultipart20230804-1-9h769v_html_9fb3fc1e199599ed.gif)Model2LossTable

1.
# Acknowledgements

The authors express their gratitude to Dr.Marwan Al Torki, teaching assistants, and the academiccommunity for their invaluable contributions and support throughout the project. They also acknowl-edge the guidance, inspiration, and feedback provided by researchers.Finally, they appreciate thecollectiveeffortsofallindividualsinvolvedinthesuccessfulcompletionoftheproject.

![](RackMultipart20230804-1-9h769v_html_60e09d349131afda.jpg)

Figure1:DistributionofAnswerTypes.

1
