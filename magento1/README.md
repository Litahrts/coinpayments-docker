<h4>Install</h4>
<ol>
    <li>git clone</li>
    <li>cd magento1</li>
    <li>docker-compose build</li>
    <li>docker-compose up</li>
    <li>docker exec -it 'container id' install-sampledata (WARNING! run befor install-magento)</li>
    <li>docker exec -it 'container id' install-magento</li>
    <li>Go To admin and activate coinpayments checkout method</li>
</ol>
