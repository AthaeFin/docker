    riippuvuudet:
      pkg.installed:
        - names:
          - ca-certificates
    
    docker_key:
      file.managed:
        - name: /etc/apt/keyrings/docker.asc
        - source: https://download.docker.com/linux/debian/gpg
        - makedirs: True
        - user: root
        - group: root
        - mode: 644
        - skip_verify: True
        - require:
          - pkg: riippuvuudet
    
    docker_repo:
      file.managed:
        - name: /etc/apt/sources.list.d/docker.list
        - contents: |
            deb [arch={{ grains['osarch'] }} signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian {{ grains['lsb_distrib_codename'] }} stable
        - user: root
        - group: root
        - mode: 644
        - require:
          - file: docker_key
    
    updates:
      pkgrepo.managed:
        - name: deb [arch={{ grains['osarch'] }} signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian {{ grains['lsb_distrib_codename'] }} stable
        - file: /etc/apt/sources.list.d/docker.list
        - refresh: True
        - require:
          - file: docker_repo
    
    docker:
      pkg.installed:
        - names:
            - docker-compose
            - docker-ce
            - docker-ce-cli
            - containerd.io
            - docker-buildx-plugin
            - docker-compose-plugin
        - require:
          - pkgrepo: updates
