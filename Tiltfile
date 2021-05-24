# -*- mode: Python -*-
load('ext://restart_process', 'docker_build_with_restart')

compile_cmd = 'CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o build/main .'
if os.name == 'nt':
  compile_cmd = 'build.bat'

local_resource(
  'example-compile',
  compile_cmd,
  deps=['./main.go'],
  )

#docker_build_with_restart('example-image', '.', entrypoint=['/app/build/main'], dockerfile='deployments/Dockerfile', only=['./build'], live_update=[sync('.build', '/app/build'),],)

docker_build('example-image', '.', dockerfile='deployments/Dockerfile', only=['./build'])

k8s_yaml('deployments/c_source.yaml')
k8s_kind('ContainerSource', image_json_path='{.spec.template.spec.containers[0].image}')
