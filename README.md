# C# Live Project
Repo for my C# Live Project at The Tech Academy. It utilizes the C# language and the .NET framework. Visual Studio was the IDE that was used.

## Overview
My task for this project was to work alongside my fellow classmates to build a website for a Theater in Portland, OR.
- [Home Page](#home-page)
- [Calendar Events](#calendar-events)
- [Rental Requests](#rental-requests)
- [Expired Requests](#expired-requests)

## Home Page
![Home Page GIF](/GIFs/homePage.gif)
### Code
- [Template](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/templates/anime_reviews_create.html)
- [Form](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/forms.py)
- [Model](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/models.py)
- View:
```cs
def anime_reviews_create(request):
    form = NewAnime(request.POST or None)
    if form.is_valid():
        form.save()
        return redirect('anime_reviews_create')
    else:
        print(form.errors)
        form = NewAnime()
        context = {'form': form}
    return render(request, 'anime_reviews_create.html', context)
```

## Calendar Events
![Calendar Events GIF](/GIFs/calendarEvents.gif)
### Code
- [Template](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/templates/anime_reviews_edit.html)
- View:
```cs
def anime_reviews_edit(request, pk):
    item = get_object_or_404(Anime, pk=pk)
    form = NewAnime(data=request.POST or None, instance=item)
    if request.method == 'POST':
        if form.is_valid():
            form2 = form.save(commit=False)
            form2.save()
            return redirect('anime_reviews_view')

        else:
            print(form.errors)
    else:
        return render(request, 'anime_reviews_edit.html', {'form': form, 'item': item})
 ```

## Rental Requests
![Rental Requests GIF](/GIFs/rentalRequests.gif)
### Code
- [Template](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/templates/anime_reviews_delete.html)
- View:
```cs
def anime_reviews_delete(request, pk):
    item = get_object_or_404(Anime, pk=pk)
    if request.method == 'POST':
        item.delete()
        return redirect('anime_reviews_view')
    context = {'item': item}
    return render(request, 'anime_reviews_delete.html', context)
 ```

## Expired Requests
![Expired Requests GIF](/GIFs/expiredRequests.gif)
### Code
- [Template](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/templates/anime_reviews_api.html)
- [Jikan API](https://rapidapi.com/theapiguy/api/jikan1)
- View:
```cs
def anime_reviews_api(request):

    resultList = []

    if request.method == 'POST':
        url = "https://jikan1.p.rapidapi.com/search/anime"

        querystring = {"q": request.POST['searchTerm']}

        headers = {
            'x-rapidapi-host': "jikan1.p.rapidapi.com",
            'x-rapidapi-key': "daa3c3b9d7mshaa4e1ceb0660c2dp1caa26jsn9495166557eb"
        }

        response = requests.request("GET", url, headers=headers, params=querystring)

        result = json.loads(response.text)

        for i in result['results']:
            url = i['url']
            img_url = i['image_url']
            title = i['title']
            resultArray = (url, img_url, title)
            resultList.append(resultArray)

    context = {
        'resultList': resultList
    }

    return render(request, 'anime_reviews_api.html', context)
 ```

## Extras
- [Static Files](https://github.com/ryanwspears/PythonLiveProject/tree/main/AnimeReviews/AnimeReviews/static)
- [URLs](https://github.com/ryanwspears/PythonLiveProject/blob/main/AnimeReviews/AnimeReviews/urls.py)
