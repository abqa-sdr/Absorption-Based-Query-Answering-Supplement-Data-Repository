# Absorption-Based-Query-Answering-Supplement-Data-Repository
Absorption-Based Query Answering Supplement Data Repository

## Documentation

The available [technical report](https://www.dropbox.com/s/yx6d6c2dxk20p3g/ABQA-Technical-Report-R2.pdf?dl=1 "Updated Technical Report") (updated) describes many details of the absorption-based query answering (ABQA) technique and the integration in the reasoning system Konclude.

## Source Code

We created an extension of the lastest official release of [Konclude](http://konclude.com "Konclude: http://konclude.com") (v0.6.2), available under the LGPL 2.1, with the absorption-based query entailment checking/answering technique. The source code for it is available at [https://www.dropbox.com/s/cr147fsyvfovnel/Konclude-ABQA-Source.tgz?dl=1](https://www.dropbox.com/s/cr147fsyvfovnel/Konclude-ABQA-Source.tgz?dl=1 "https://www.dropbox.com/s/cr147fsyvfovnel/Konclude-ABQA-Source.tgz?dl=1"). A precompiled binary for Linux 64bit (with statically linked libraries) can be found in the evaluation data (`Evaluation/Reasoners/Konclude-SPARQL/Linux64/v0.6.2-771/libs/Konclude`). Please do not yet distribute the source code since the copyright/license has to be adapted.

For compiling Konclude with the ABQA extension, the following libaries are required:
* [Qt (version 5.10 or higher)](https://www.qt.io/ "Qt: https://www.qt.io/")
* [Redland RDF Libraries](http://librdf.org/ "Redland RDF Libaries: http://librdf.org/")

In addition, a C++ compiler such as [GCC](https://gcc.gnu.org/ "GNU Compiler Collection: https://gcc.gnu.org/") is required for compiling (the dependencies and) Konclude.

For compilation, you may execute qmake for the Qt Konclude project file (`Konclude.pro`). This creates a makefile, which then allows for compiling Konclude by executing make. You may have to specify where the Qt and the Redland RDF Libraries can be found. For the latter, you can edit the Konclude project file (`Konclude.pro`), which refers by default to the precompiled Redland RDF Libraries in `/External/librdf/Linux/x64/lib/release/`. For specifying it for Qt, you may use the following compilation commands (assuming that Qt is installed in `/data/Qt/Qt-5.10-static`):
```
export PATH=/data/Qt/Qt-5.10-static/bin:$PATH
export QTDIR=/data/Qt/Qt-5.10-static
export QTINC=/data/Qt/Qt-5.10-static/include
export QTLIB=/data/Qt/Qt-5.10-static/lib

qmake -r CONFIG+=release CONFIG+=x86_64 CONFIG-=debug CONFIG+=static -spec /data/Qt/Qt-5.10-static/mkspecs/linux-g++-64 Konclude.pro

make -j
```

A static version of Qt can be obtained as follows:
```
wget http://download.qt.io/official_releases/qt/5.10/5.10.1/single/qt-everywhere-src-5.10.1.tar.xz
tar -xJf qt-everywhere-src-5.10.1.tar.xz
cd qt-everywhere-src-5.10.1
./configure -static -prefix /data/Qt/Qt-5.10.1-static -extprefix /data/Qt/Qt-5.10.1-static -no-opengl -nomake examples -nomake tests -opensource -platform linux-g++-64 -no-icu
make -j
make install
```

## Evaluation Framework and Data

The evaluation framework and data can be downloaded at [https://www.dropbox.com/s/mpd3knqhknxzeyr/ABQA-Evaluation-Framework-icu-fix.tgz?dl=1](https://www.dropbox.com/s/mpd3knqhknxzeyr/ABQA-Evaluation-Framework-icu-fix.tgz?dl=1 "Evaluation Framework and Data: https://www.dropbox.com/s/5k941cazomjvpef/ABQA-Evaluation-Framework.tgz?dl=1") (updated for removing icu dependency) and includes the following data:
* Linux 64bit binaries for all evaluated reasoners
* the evaluated ontologies (in OWL2/XML and RDF Turtle files) and queries (in SPARQL syntax)
* an evaluator program that is capable of conducting the evaluations

For the evaluations, we used Konclude’s integrated evaluator (i.e., the "evaluator" is also a binary of Konclude). The evaluations can be executed on a Linux 64bit system by running the provided scripts (`run-entailment-checking-experiments.sh` and `run-query-answering-experiments.sh`). Note that each experiment may take more than 10 hours (depending on the hardware). The reasoners should not require more than 10/15 GB, but we configured/used 32 GB as memory limit (it should also run on hardware with less RAM). Also note that port 8880 must be free since it is used by the reasoners for their SPARQL endpoints. 

All reasoner responses are saved into subdirectories of the `Evaluation/Responses` folder, but the evaluator is analysing all responses in the end and summarises some results in the `Evaluation/Analyses` directory. An overview over the summarised results can be found in the files `Evaluation/Analyses/A-000/Querying-SPARQL-PAGOdATests-Single-Fast001/Linux64/OnlyDirectoryGrouped-DirectlyLinkedNavigationOverview.html`
and `Evaluation/Analyses/A-000/Querying-SPARQL-KoncludeTests-Single-Fast001/Linux64/OnlyDirectoryGrouped-DirectlyLinkedNavigationOverview.html`, which should provide links to the most interesting charts and tables.

You may want to run the script `run-analyses-webserver.sh` to start a small web sever that exposes the `Evaluation/Analyses` directory on port [8888](http://localhost:8888 "http://localhost:8888") such that you can easily navigate through the summarised results.
The most interesting pages are probably as follows (assuming the web server has been started on the localhost):
* [query-answering-overview-page](http://localhost:8888/A-000/Querying-SPARQL-PAGOdATests-Single-Fast001/Linux64/OnlyDirectoryGrouped-DirectlyLinkedNavigationOverview.html) lists the most interesting charts/tables for the query answering evaluation
* [complex-querying-accumulated-time-table-vertical-chart](http://localhost:8888/A-000/Querying-SPARQL-PAGOdATests-Single-Fast001/Linux64/AccumulatedTimeComparison/DirectoryGrouped/Q-000/complex-querying-accumulated-time-table-vertical-chart.html) shows the accumulated query answering time for each reasoner over all queries/ontologies
* [query-entailment-checking-overview-page](http://localhost:8888/A-000/Querying-SPARQL-KoncludeTests-Single-Fast001/Linux64/OnlyDirectoryGrouped-DirectlyLinkedNavigationOverview.html) lists the most interesting charts/tables for the query entailment checking evaluation
* [time-mmm-sorted-ascending-by-requests-table-vertical-chart](http://localhost:8888/A-000/Querying-SPARQL-KoncludeTests-Single-Fast001/Linux64/ComplexQueryingTimeComparison/DirectoryGrouped/Q-000/Queries/SPARQL/KoncludeTestQueries/SpecialSelections/AbsorptionBasedQueryAnswering/KoncludeTestQueries/EntailmentTestingOntologies/time-mmm-sorted-ascending-by-Requests-table-vertical-chart.html) shows the query entailment checking times for each query

Note that the query entailment checking evaluation tests all reasoners, but currently only our extension of Konclude correctly processes the SPARQL ASK queries, i.e., the response times of other reasoners are not meaningful. Also note that all results are "cached" in files, i.e., if you stop and restart the evaluation, then it will continue where you stopped it. If you want to start the evaluation from scratch, then you have to delete the responses and analyses, e.g., by executing the `clean.sh` script.
