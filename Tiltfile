load('ext://cert_manager', 'deploy_cert_manager')
load('ext://helm_resource', 'helm_resource', 'helm_repo')
load('ext://namespace', 'namespace_create')

deploy_cert_manager()

namespace_create('sunk')

# -- CRDs --
helm_repo('mariadb-operator', 'https://mariadb-operator.github.io/mariadb-operator', labels=['crds'])
helm_resource(
    'mariadb-operator-crds',
    'mariadb-operator/mariadb-operator-crds',
    labels=['crds'],
    pod_readiness='ignore',
    resource_deps=['mariadb-operator'],
)
helm_resource(
    'slurm-operator-crds',
    'oci://ghcr.io/slinkyproject/charts/slurm-operator-crds',
    labels=['crds'],
    pod_readiness='ignore',
)

# -- Operators --
helm_resource(
    'mariadb-operator-deploy',
    'mariadb-operator/mariadb-operator',
    namespace='mariadb',
    flags=['--create-namespace', '--skip-crds', '--set', 'crds.enabled=false'],
    labels=['operators'],
    resource_deps=['mariadb-operator-crds'],
)
helm_resource(
    'slurm-operator-deploy',
    'oci://ghcr.io/slinkyproject/charts/slurm-operator',
    namespace='slinky',
    flags=['--create-namespace'],
    labels=['operators'],
    resource_deps=['slurm-operator-crds'],
)

flags = ['--values=chart/values.local.yaml'] if os.path.exists('chart/values.local.yaml') else []

# -- Sunk chart --
helm_resource(
    'sunk',
    'chart',
    namespace='sunk',
    flags=['--create-namespace'] + flags,
    labels=['sunk'],
    deps=['chart'],
    resource_deps=['mariadb-operator-deploy', 'slurm-operator-deploy'],
)
