# IMAGEN DE DESARROLLO
FROM python:3.8 as development


# UPDATE and Install
RUN apt-get update && apt-get install -q -y \
    vim \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Users
ARG USER=dev
ARG IDUSER=1000
ARG GROUP=development
ARG IDGROUP=${IDUSER}

RUN groupadd -g ${IDGROUP} ${GROUP} \
    && useradd -m -u ${IDUSER} -g ${IDGROUP} -s /bin/bash ${USER} \
    && echo ${USER} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USER} \
    && chmod 0440 /etc/sudoers.d/${USER}


# Image cmd
ARG CODE=/work
WORKDIR ${CODE}
COPY ./src .
RUN chown -Rf ${USER} ${CODE}/ \
    && chmod 744 -R ${CODE}/ \
    && chgrp -R ${GROUP} ${CODE}/

USER ${USER}
ENTRYPOINT ["/bin/bash"]
CMD ["-c -l"]


FROM python:3.8-alpine as prod
RUN groupadd -g ${IDGROUP} ${GROUP} \
    && useradd -m -u ${IDUSER} -g ${IDGROUP} -s /bin/bash ${USER}
USER ${USER}
WORKDIR ${CODE}
COPY --from=development ${CODE} .
ENTRYPOINT ["python"]
CMD ["hello.py"]
