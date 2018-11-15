<h4>Prepare</h4>
Download magento-sample-data (version 1.9.2.4) from <a href="https://magento.com/tech-resources/download#download1759">magento sample data</a>
and place to magento1/bin dir with name <strong>magento-sample-data.tar.gz</strong>.
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
