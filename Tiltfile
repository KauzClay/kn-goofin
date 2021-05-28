# -*- mode: Python -*-
load('ext://restart_process', 'docker_build_with_restart')

#Container Source

compile_csource = 'CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o build/csource .'
if os.name == 'nt':
  compile_cmd = 'build.bat'

local_resource(
  'compile-csource',
  compile_csource,
  deps=['./container_source/container_source.go'],
  )

#docker_build_with_restart('example-image', '.', entrypoint=['/app/build/main'], dockerfile='deployments/Dockerfile', only=['./build'], live_update=[sync('.build', '/app/build'),],)

docker_build('csource-image', '.', dockerfile='container_source/Dockerfile', only=['./build'])

k8s_yaml('container_source/c_source.yaml')
k8s_kind('ContainerSource', image_json_path='{.spec.template.spec.containers[0].image}')

#Sequence

compile_transformer = 'CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o build/transformer .'
if os.name == 'nt':
  compile_cmd = 'build.bat'

local_resource(
  'compile-transformer',
  compile_transformer,
  deps=['./tansformer/transformer.go'],
  )

docker_build('transformer-image', '.', dockerfile='transformer/Dockerfile', only=['./build'])

k8s_yaml('transformer/transformer.yaml')
k8s_kind('Service', image_json_path='{.spec.template.spec.containers[0].image}')
