# README

Welcome to the MEFL repository! Thank you for your support of this project!! This repository contains the implementation source code of MEFL, as well as experimental result data for MEFL and baselines (including their various variants).

## Runtime Environment
MEFL's main working environment:
- `python3: 3.10.12`
- `gcov: 13.1.0`
- `gcc: 13.1.0`
- `c++: 13.1.0`
- `Ubuntu: 22.04.3`
- `CPU: Intel Xeon CPU E5-2680 v4@2.40GHz with 251G memory`
- `llvm-IRMutator source code version: 15.0.4`

## Introduction to MEFL's Main Files
- `my-src`: MEFL source code directory
  - `my-src/config`: Path configuration directory
    - `config.ini`: Contains path information for multiple working files/directories. Note that `mefl_path` doesn't need manual configuration. `setup.py` will use `os.getcwd` to get the current directory and modify `mefl_path` during runtime.
  - `auto-exps.py`: Performs bug isolation for multiple data points
  - `main.py`: Called by `auto-exps.py` to execute bug isolation for a single data point
  - `utils.py`: Defines various utility functions
  - `setup.py`: Performs necessary configuration work before isolation, such as modifying paths in `config.ini`
  - `eval.py`: Analyzes coverage information of all data points in `exp-results` (this directory will be generated after `setup.py`) to obtain `Top-n/MAR/MFR` metric values
- `llvm-15-bin`: Contains necessary binary executables for MEFL's operation, such as `llvm-my-fuzzer` (the mutator implemented using LLVM's three mutation operators).
- `experiment-ready`: Dataset directory
  - `experiment-ready/llvmbugs`
    - `1.fail`: Input IR file (binary format)
    - `fail.c`: Input C file (`baseline` techniques require `C-level` input)
    - `locations`: Various information about this bug
  - `experiment-ready/llvm-versions`: Directory for different version compilers, initially empty. `auto-exps.py` contains code for building `llvm`.
- `experiment_results`: Contains isolation result files for each data point from `MEFL, ETEM, ODFL, RecBi` and related method variants

## How to run MEFL
```shell
# step 1: clone the project
git clone git@github.com:watermelonAnonymous/AnonymousSubmit.git
# step 2: Enter working directory
cd ./AnonymousSubmit/MEFL
# step 3: Perform necessary configurations
python3 setup.py
# step 4: Run bug isolation program
python3 auto-exps.py
# step 5: Obtain evaluation results for metrics
python3 eval.py
```

Friendly reminder:
- Running one data point takes approximately 1 hour (excluding time to build the faulty LLVM version)
- `auto-exps.py` contains logic for building compilers corresponding to each data point's faulty LLVM version. Since coverage information needs to be collected, the build process involves instrumentation and requires significant space (about 50G per build, totaling approximately 4.6T - please reserve sufficient space). Additionally, building also takes considerable time (in the author's experimental environment, building with make -j64 takes about 15 minutes per build).



# More details about our dataset



|bugID|buggy compiler commit ID|buggy opti-level|#covFiles|#files|buggy files|
|--|--|--|--|--|--|
|69097|ca18e21951a89f3115694fc64cd64a4b06cd5873|-O2|797|60252|ScalarEvolution.cpp/LoopVectorize.cpp|
|66986|0d95b20b63d7acc459dc0b2a7b2e4f9924c0adce|-O3|790|60178|IndVarSimplify.cpp|
|66066|6ed152aff4aab6307ecaab64a544d0524ea5f50e|-Os|785|60288|ScalarEvolution.cpp|
|68260|3ddd1ffb721dd0ac3faa4a53c76b6904e862b7ab|-O2|787|60009|ScalarEvolution.cpp|
|64669|f12a5561b2cbfae384c9a31293938ee2acea79fd|-O1|737|61301|InstructionCombining.cpp|
|64598|84bcfa0e1b34938d1d11a44e9e17c6e222dd2f42|-O2|792|60502|GVN.cpp/Local.cpp|
|64345|497966f7f2bbc33f79725c4aba12e4089419e3b6|-O2|773|61452|InstructionSimplify.cpp|
|64333|1fc425380e9860a6beb53fa68d02e7fb14969963|-O3|796|60250|ScalarEvolution.cpp/ScalarEvolutionExpander.cpp|
|64259|ad7f02010f32bff28fec139e103ad0240e160aa9|-O1|764|61439|InstructionCombining.cpp|
|60944|1b9b4f3bfa8bc7ed7dddd30dd30a07676891bedb|-O2|794|59230|ScalarEvolution.cpp|
|58401|333246b48ea4a70842e78c977cc92d365720465f|-O2|788|56734|InstructionCombining.cpp|
|63926|46333f71f8e0d6444a9b2c9e063aedb83ebb9735|-O3|784|61165|ScalarEvolution.cpp/ScalarEvolutionExpander.cpp|
|63763|99074aafc31593c9935da483edab1333d6ce5a5b|-O3|786|61137|ScalarEvolution.cpp/ScalarEvolutionExpander.cpp|
|63727|99074aafc31593c9935da483edab1333d6ce5a5b|-O3|779|61137|ScalarEvolution.cpp/ScalarEvolutionExpander.cpp|
|64047|a5bba98a58b7406f81629d3942e03b1eff1e2b33|-O2|785|61306|LoopVectorize.cpp|
|63327|53d405762219d798286b99c297098a14643440fe|-O1|736|60578|InstCombineCompares.cpp|
|62088|01b5c60d9ae09b666459330b62b78decdaae5d94|-Os|787|59808|InstCombineSelect.cpp|
|61312|15d5c59280c9943b23a372ca5fdd8a88ce930514|-O1|768|59320|InstructionSimplify.cpp|
|59704|ff25800d4ba0b577a44dc918da7a1fb3c29fdb13|-O1|783|58488|InstCombineAndOrXor.cpp|
|58725|bc886e9b587b9d009f49b12eaaa9ebc1c71905a1|-O1|750|57167|InstCombineMulDivRem.cpp|
|58340|0c1a3da8ea1f0e024ebfd85c7532926f26c6bde5|-O3|784|56618|LoopUnroll.cpp|
|57899|8f19de848b968bfdd237bdb6ffb65e7412bb6a0c|-O1|777|56403|InstCombineCasts.cpp|
|62175|e779d54d2137c918572f4a70bf5c9341500fab6b|-O1|767|59852|InstCombineMulDivRem.cpp|
|58441|faf0e1fbf90f14a92042a83f6cb1239791674412|-Os|786|56791|LoopFlatten.cpp|
|66484|5db2735af91be3bfb2fcb06bf8e1ac5f7d1e4812|-O2|749|48154|Local.cpp|
|69096|45ecfed6c636d06f76bca0a44803e945cdae9506|-O2|772|52127|BasicAliasAnalysis.cpp|
|70547|b22ffc7b98f8700d7d480127ff1c3683a6dac6e5|-O2|787|53928|BasicAliasAnalysis.cpp/CaptureTracking.cpp|
|54228|17a68065c378da74805e4e1b9a5b78cc9f83e580|-Os -enable-constraint-elimination|791|53338|ConstraintElimination.cpp|
|54224|30f1cef86b56e0bae5b78ceed05a7fdbad4959a9|-Os -enable-constraint-elimination|774|53335|ConstraintElimination.cpp|
|54217|d5d03135a7160586e2b09dc47c7021379252fbbd|-Os -enable-constraint-elimination|776|53353|ConstraintElimination.cpp|
|63336|09d6ee765780837d5156ac81f968465bdcec73ba|-O3 -enable-newgvn|780|60588|NewGVN.cpp|
|63337|09d6ee765780837d5156ac81f968465bdcec73ba|-O3 -enable-newgvn|774|60588|NewGVN.cpp|
|56039|e7c72d69ac0d504f52bed643b298b50904c6b387|-O3 -enable-newgvn|783|54850|NewGVN.cpp|
|54235|17a68065c378da74805e4e1b9a5b78cc9f83e580|-Os -enable-constraint-elimination|773|53338|ConstraintElimination.cpp|
|54112|53602e4c704f7461426d3981dcdca92e892eca99|-Os|792|53266|LoopSimplifyCFG.cpp|
|54023|e0f1dd018e0f94a7d694bc615975c3a7d26d9e50|-O1|783|53233|LoopSimplifyCFG.cpp|
|52722|9912bed7306f0157b6425f233eec56d04bb536cf|-O1|776|52350|LoopSimplifyCFG.cpp|
|51618|2410fb4616b2c08bbaddd44e6c11da8285fbd1d3|-O1|757|51748|IndVarSimplify.cpp|
|51136|ed7b37155b48715dc5b2ec8c52571ce754cf7a80|-Os|776|51981|IVDescriptors.cpp|
|50727|82ca845b479359057410c14600f586fd7c86fd5b|-O1|740|50735|SimplifyCFG.cpp|
|50469|5df48493f08930ad05d9746c65fd53e5131a08d0|-O1|753|50414|SimplifyCFG.cpp|
|50430|20176bc7dd3f431db4c3d59b51a9f53d52190c82|-O3|755|50271|SimpleLoopUnswitch.cpp|
|50387|4a3b0556536d5a2555f7a19f953f0eec0f79f1a9|-O3|753|50243|SimpleLoopUnswitch.cpp|
|50296|b9c24257c7b4da398798934ffefdd30015152180|-Os|732|50165|InstructionSimplify.cpp|
|50291|0c96a92d8666b8eb69eb1275aed572f857182d9a|-Os|753|50147|InstructionSimplify.cpp|
|50288|0c96a92d8666b8eb69eb1275aed572f857182d9a|-Os|745|50147|InstructionSimplify.cpp|
|50250|043ce4e6bdd376ff460d78446d1a6b94c6e0f18c|-O2|746|50074|InstCombineCompares.cpp|
|50246|043ce4e6bdd376ff460d78446d1a6b94c6e0f18c|-O2|741|50074|InstCombineCompares.cpp|
|49965|61a2d6bfe48cf3da4b95d1383bf866690287f8e8|-O2|752|49810|DeadStoreElimination.cpp|
|49931|92ce29ee45b26513a9f4e42c6f287f43cb3de238|-O3|754|49789|LoopSimplifyCFG.cpp|
|49597|f7294ac8093a2fbd8c00254580eaac6c4e1f7b24|-O2|737|49224|GlobalOpt.cpp|
|49005|f860187ea6e9b30e1ecf74784f0af0e0c9ecc01c|-O2|734|48498|GlobalOpt.cpp|
|44705|f6b2c003f360c3d0dcfe13d751805643f229f79b|-O2|752|42865|SimplifyIndVar.cpp|
|48813|93afc3452cd4ebdb145e309f4d82f82e2f496479|-O1|762|47736|TargetLowering.cpp|
|50519|599b2f00370ee79e812d2776f2af57fae36d02e9|-O3|787|50498|X86ISelDAGToDAG.cpp|
|56103|be6af89f85ebd04646b5704301470f02b70a0447|-O1|813|54945|X86InstrInfo.cpp|
|108722|987087df90026605fc8d03ebda5a1cd31b71e609|-O1|822|64587|X86ISelLowering.cpp|
|82243|dce77a357948709e335910ddc07f9c3f2eb2ac4b|-Os|785|64871|ScalarEvolutionExpander.cpp/SimplifyIndVar.cpp|
|87630|4f19f15a601a5761b12c9c66d99d97dbc89ef90d|-O3|785|65699|SLPVectorizer.cpp|
|87534|07a566793b2f94d0de6b95b7e6d1146b0d7ffe49|-O1|749|65680|CloneFunction.cpp|
|85536|74d1a40915834cbf0629f8d34a7265734d4d9073|-O1|759|65412|InstructionCombining.cpp|
|129244|42cbceb0f0160d67145723613fda325dbd129308|-O2|775|65682|SLPVectorizer.cpp|
|121110|5287299f8809ae927a0acafb179c4b37ce9ff21d|-Os|814|69751|VectorCombine.cpp|
|88103|51089e360e37962c7841fe0a494ba9fb5368bab2|-O3|785|65741|SLPVectorizer.cpp|
|114879|d77067d08a3f56dc2d0e6c95bd2852c943df743a|-O2 -unroll-full-max-count=1|773|63773|ScalarEvolution.cpp|
|131465|04cd06906288dcb148de37d7c3e6c009c3e3b726|-O2|796|66578|ScalarEvolution.cpp|
|102351|67782d2de5ea9c8653b8f0110237a3c355291c0e|-O1|744|64289|Local.cpp|
|80113|20737825c9122b6e0a8912731cfa7e0558fe025d|-O2|779|64676|BDCE.cpp|
|116483|254da5ab8bce846bcbac9862f31c1891d8feea44|-O2|799|67412|ScalarEvolution.cpp|
|79743|408dce82016463dcb5026b2ddfc62174970a88e9|-O2|785|64340|SLPVectorizer.cpp|
|119646|ebe741fad07e3fda388d0fa44f256a07429cce6a|-Os|801|69624|DeadStoreElimination.cpp|
|94897|338cbfef03e0ab58d7b52f3301928c58b194a1b4|-O1|776|66451|InstCombineCompares.cpp|
|121745|df4a615c988f3ae56f7e68a7df86acb60f16493a|-O2|808|69910|VPlanTransforms.cpp|
|79861|febb4c42b192ed7c88c17f91cb903a59acf20baf|-O3|792|64674|SimplifyIndVar.cpp|
|119173|6d6eea92e36c301e34a7ec11b2a40e3080f79f53|-O2|801|68216|LoopVectorize.cpp|
|84807|0f501c30b9601627c236f9abca8a3befba5dc161|-O3|784|65283|ArgumentPromotion.cpp|
|108698|223e2efa5e886502a9467b7ef700ebce9b7886e8|-O2|804|67894|VectorCombine.cpp|
|98838|52139d8f9a4e3f595ca552393d62ba06b0bc082c|-O3|796|66939|SLPVectorizer.cpp|
|98139|765e2f9a8de27cc8fd8c75540844e9630d8229ad|-O2|792|66879|InstCombineSimplifyDemanded.cpp|
|124275|ddd2f57b29661f21308eec0400fa92a6d075b0c6|-O1|766|70274|ValueTracking.cpp|
|116553|5b927130b0e15a442a6ed171f43a612e6a40bbcd|-O2|792|66883|ConstraintElimination.cpp|
|122496|ab9a80a3ad78f611fd06cd6f7215bd828809310c|-O2|815|69993|LoopVectorize.cpp/VPlanTransforms.cpp|
|71700|6eb97f03802a219997af4b615a2afae339cb674d|-O1|758|63172|InstructionCombining.cpp|
|72831|d53b3df570e359d175d6e7a825ad1a02f9bc80a3|-O2|784|61645|BasicAliasAnalysis.cpp|
|74739|e4710872e98a931c6d5e89bc965b778746ead2c0|-O1|731|63706|InstructionCombining.cpp|
|75298|19918ac34dc5d304ec6ad413ceae1d4394abe28f|-Os|777|63840|VPlanRecipes.cpp|
|76162|8773c9be3d9868288f1f46957945d50ff58e4e91|-O1|739|64146|InstCombineCompares.cpp|
|76789|8f3d16905d75b07a933d01dc29677fe5867c1b3e|-O1|750|50016|BasicAliasAnalysis.cpp|
|115149|461274b81d8641eab64d494accddc81d7db8a09e|-O3|788|64659|InstCombinePHI.cpp|
|124387|a4bf6cd7cfb1a1421ba92bca9d017b49936c55e4|-O2|777|67155|InstCombineSimplifyDemanded.cpp|
|70507|4def99e642806f3f8e94cb2aa9356d63cd233dd0|-O2|790|58192|SLPVectorizer.cpp|
|70509|a66051c68a43af39f9fd962f71d58ae0efcf860d|-O1|779|62921|InstCombineCompares.cpp|
|70510|98e016d99732dc8fef8cfd61d6ce1edd042309a1|-O2|791|62558|ConstraintElimination.cpp|
|70470|8ff14223536d6645d7c92b4488e3eb676e548cb1|-O1|769|62923|InstCombineCompares.cpp|
|63611|36999318f09122fa41baff838f46c753db078275|-Os|777|60768|GVN.cpp|


