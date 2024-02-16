1 задание
using System.Net;
using System.Net.Http;
using NUnit.Framework;

[TestFixture]
public class APITests
{
    private HttpClient _httpClient;
    private string _apiBaseUrl = "- https://petstore.swagger.io";

    [SetUp]
    public void Setup()
    {
        _httpClient = new HttpClient();
    }

    [TearDown]
    public void TearDown()
    {
        _httpClient.Dispose();
    }

    [Test]
    public void TestSuccessfulRequest()
    {
        // меняем "/some_endpoint" на путь API
        string endpoint = "/some_endpoint";
        HttpResponseMessage response = _httpClient.GetAsync(_apiBaseUrl + endpoint).Result;

        // смотрим что состояник HTTP равен 200 (Успешный запрос)
        Assert.AreEqual(HttpStatusCode.OK, response.StatusCode);
    }
}

2 задание
using NUnit.Framework;
using System.Net.Http;
using System.Net;
using System.Threading.Tasks;

[TestFixture]
public class APITests
{
    private const string apiUrl = "https://api.example.com/some_endpoint";

    [Test]
    public async Task TestSuccessfulRequest()
    {
        using (HttpClient client = new HttpClient())
        {
            HttpResponseMessage response = await client.GetAsync(apiUrl);

            // смотрим чтоб запрос был успешным (код состояния HTTP 200)
            Assert.AreEqual(HttpStatusCode.OK, response.StatusCode);
            string responseData = await response.Content.ReadAsStringAsync();
            Assert.IsTrue(responseData.StartsWith("{"));
            Assert.IsTrue(responseData.EndsWith("}"));
        }
    }
}

3 задание
using NUnit.Framework;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

[TestFixture]
public class APITests
{
    private const string apiUrl = "https://api.example.com/some_endpoint";

    [Test]
    public async Task TestInvalidParameters()
    {
        // подготовка запроса с неверными параметрами, пример: добавление неправильного параметра в URL
        string invalidUrl = apiUrl + "?invalid_param=1";

        using (HttpClient client = new HttpClient())
        {
            HttpResponseMessage response = await client.GetAsync(invalidUrl);

            // проверяем что запрос завершился с ожидаемым кодом состояния HTTP (400 Bad Request)
            Assert.AreEqual(HttpStatusCode.BadRequest, response.StatusCode);
        }
    }
}
