# 1. cert-managerのインストール

kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.0/cert-manager.yaml

# 2. Webhookの準備ができるまで待機（これが重要！）

kubectl wait --for=condition=Available --timeout=300s deployment/cert-manager-webhook -n cert-manager

※ podがrunnningでも初期化が完了していないかもしてない

# 3. Webhook用の証明書(Secret)が作成されているか確認

kubectl get secret cert-manager-webhook-ca -n cert-manager

# 4. 実際にAPIが「生きているか」を軽く叩いてみる

kubectl get clusterissuers

# cert-managerのAPIが正常に応答するか確認
# これが通れば、証明書の発行や他Podの作成が安全に行える証拠です
kubectl get issuers.cert-manager.io --all-namespaces

# 事前作業Webhookを2台に増やす
kubectl scale deployment cert-manager-webhook -n cert-manager --replicas=2