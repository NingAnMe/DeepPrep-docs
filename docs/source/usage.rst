-----------
Usage Notes
-----------

===============
The BIDS format
===============

The DeepPrep workflow takes as principal input the path of the dataset that is to be processed.
The input dataset is required to be in valid BIDS format, it is possible to include only a T1w
image to run only the anat part,or just include a BOLD image and only run the func part
(the complete Recon needs to be specified).We highly recommend that you validate your dataset
with the free, online `BIDS Validator`_.

.. _BIDS Validator: http://bids-standard.github.io/bids-validator/

Further information about BIDS and BIDS-Apps can be found at the `NiPreps portal`_.

.. _NiPreps portal: https://www.nipreps.org/apps/framework/


======================
Command-line arguments
======================

DeepPrep: Deep learning empowered preprocessing workflow v23.1.0:

.. code-block:: none

   usage: deepprep-docker [bids_dir] [output_dir] [{participant}] [--bold_task_type TASK_LABEL]
                          [--fs_license_file PATH] [--participant-label PARTICIPANT_LABEL [PARTICIPANT_LABEL ...]]
                          [--subjects_dir PATH] [--executor {local cluster}]
                          [--anat_only] [--bold_only] [--bold_sdc] [--bold_confounds]
                          [--bold_surface_spaces '[fsnative fsaverage fsaverage6 ...]']
                          [--bold_template_space {MNI152NLin6Asym MNI152NLin2009cAsym}] [--bold_template_res {02 03...}]
                          [--device { {auto 0 1 2...} cpu}] [--gpu_compute_capability {8.6}]
                          [--cpus 10] [--memory 5]
                          [--ignore_error] [--resume]




======================
The FreeSurfer license
======================
DeepPrep is compatible with FreeSurfer tools, thus requires a valid license.

    To obtain a FreeSurfer license, simply register for free at
    https://surfer.nmr.mgh.harvard.edu/registration.html.

Pleas make sure that a valid license file is passed into the command.
For example, if the license is stored in the ``$HOME/freesurfer/license.txt`` file on
the host system, the ``<fs_license_file>`` in command ``-v <fs_license_file>:/fs_license.txt`` should be replaced with the valid path: ::

    $ -v $HOME/freesurfer/license.txt:/fs_license.txt


=====================
Sample Docker command
=====================
.. code-block:: bash
    :linenos:

    $ docker run -it --rm --gpus all \
                 -v <bids_dir>:/input \
                 -v <output_dir>:/output \
                 -v <fs_license_file>:/fs_license.txt \
                 ninganme/deepprep:v23.1.0 \
                 /input \
                 /output \
                 participant \
                 --bold_task_type rest \
                 --fs_license_file /fs_license.txt

**Let's dig into the mandatory commands**
    + ``<bids_dir>`` - refers to the directory of the input dataset, which should be in `BIDS format`_.
    .. _BIDS format: https://bids-specification.readthedocs.io/en/stable/index.html
    + ``<output_dir>`` - refers to the directory for the outputs of DeepPrep.
    + ``<fs_license_file>`` - the directory of a valid FreeSurfer License.
    + ``deepprep:v23.1.0`` - the latest version of the Docker image. One can specify the version by ``deepprep:<version>``.
    + ``participant`` - refers to the analysis level.
    + ``--bold_task_type`` - the task label of BOLD images (i.e. ``rest``, ``motor``).

**Dig further (optional commands)**
    + ``--subjects_dir`` - the output directory of *Recon* files, default is ``<output_dir>/Recon``.
    + ``--participant_label`` - the subject id you want to process, otherwise all the subjects in the ``<bids_dir>`` will be processed.
    + ``--anat_only`` - with this flag, only the *anatomical* images will be processed.
    + ``--bold_only`` - with this flag, only the *functional* images will be processed, where *Recon* files are pre-requested.
    + ``--bold_sdc`` - with this flag, susceptibility distortion correction (SDC) will be applied.
    + ``--bold_confounds`` - with this flag, confounds will be generated.
    + ``--bold_surface_spaces`` - specifies surfaces spaces, i.e. ``'fsnative fsaverage fsaverage6'``, default is ``fsaverage6``. (*Note:* the space names must be quoted using single quotation marks.)
    + ``--bold_template_space`` - specifies an available template space from `TemplateFlow`_, default is ``MNI152NLin6Asym``.
    .. _TemplateFlow: https://www.templateflow.org/browse/
    + ``--bold_template_res`` - specifies the resolution of the corresponding template space from `TemplateFlow`_, default is ``02``.
    + ``--device`` - specifies the device, default is ``auto``, which automatically select a GPU device; ``0`` specifies the first GPU device; ``cpu`` refers to CPU only.
    + ``--gpu_compute_capability`` - refers to the GPU compute capability, you can find yours `here`_.
    .. _here: https://developer.nvidia.com/cuda-gpus
    + ``--cpus`` - refers to the maximum CPUs for usage, should be integer values > 0.
    + ``--memory`` - refers to the maximum memory resources for usage, should be integer values > 0.
    + ``--ignore_error`` - ignores the errors occurred during processing.
    + ``--resume`` - allows the DeepPrep pipeline starts from the last exit point.

Quick start
-----------

Get started with a ``test_sample``, `download here`_.

.. _download here: https://github.com/NingAnMe/DeepPrep-docs/archive/refs/tags/test_sample.zip

The BIDS formatted sample contains one subject with one T1w and two bold files.

1. run with GPU (**recommended**)

.. code-block:: bash
    :linenos:

    $ docker run -it --rm --gpus all \
                 -v ~/test_sample:/input \
                 -v ~/deepprep_output:/output \
                 -v ~/license.txt:/fs_license.txt \
                 ninganme/deepprep:v23.1.0 \
                 /input \
                 /output \
                 participant \
                 --bold_task_type rest \
                 --fs_license_file /fs_license.txt

**Docker arguments**
    + ``-it`` - (optional) starts the container in an interactive mode.
    + ``--rm`` - (optional) the container will be removed when exit.
    + ``--gpus all`` - (optional) assigns all the available GPUs on the local host to the container. *This flag is highly recommended*.
    + ``-v`` - mounts your local directories to the directories inside the container. The input directories should be in *absolute path* to avoid any confusion.


2. run with CPU only

.. code-block:: bash
    :linenos:

    $ docker run -it --rm \
                 -v ~/test_sample:/input \
                 -v ~/deepprep_output:/output \
                 -v ~/license.txt:/fs_license.txt \
                 ninganme/deepprep:v23.1.0 \
                 /input \
                 /output \
                 participant \
                 --bold_task_type rest \
                 --fs_license_file /fs_license.txt \
                 --device cpu

    + ``--device cpu`` - refers to CPU only.


.. container:: congratulation

   **Congratulations! You are all set!**

