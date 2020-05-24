1. in AbstractEncoder.php line 170
2. at AbstractEncoder->process(*object*(Image), 'svg', *null*) in AbstractDriver.php line 77
3. at AbstractDriver->encode(*object*(Image), 'svg', *null*) in Image.php line 119
4. at Image->encode('svg', *null*) in Image.php line 139
5. at Image->save('/data/recruitment/public/forms/uploads/userdocuments/31386_Citizenship.svg') in UploadsController.php line 204
6. at UploadsController->update(*object*(Request), '31386')
7. at call_user_func_array(*array*(*object*(UploadsController), 'update'), *array*(*object*(Request), 'document' => '31386')) in Controller.php line 55
8. at Controller->callAction('update', *array*(*object*(Request), 'document' => '31386')) in ControllerDispatcher.php line 44
9. at ControllerDispatcher->dispatch(*object*(Route), *object*(UploadsController), 'update') in Route.php line 190
10. at Route->runController() in Route.php line 144
11. at Route->run(*object*(Request)) in Router.php line 653
12. at Router->Illuminate\Routing\{closure}(*object*(Request)) in Pipeline.php line 53
13. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in DeniedIfApprovedUser.php line 25
14. at DeniedIfApprovedUser->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
15. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
16. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in DeniedIfNotFormUser.php line 21
17. at DeniedIfNotFormUser->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
18. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
19. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in SubstituteBindings.php line 41
20. at SubstituteBindings->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
21. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
22. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in VerifyCsrfToken.php line 65
23. at VerifyCsrfToken->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
24. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
25. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in ShareErrorsFromSession.php line 49
26. at ShareErrorsFromSession->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
27. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
28. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in StartSession.php line 64
29. at StartSession->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
30. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
31. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in AddQueuedCookiesToResponse.php line 37
32. at AddQueuedCookiesToResponse->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
33. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
34. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in EncryptCookies.php line 59
35. at EncryptCookies->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
36. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
37. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in Pipeline.php line 104
38. at Pipeline->then(*object*(Closure)) in Router.php line 655
39. at Router->runRouteWithinStack(*object*(Route), *object*(Request)) in Router.php line 629
40. at Router->dispatchToRoute(*object*(Request)) in Router.php line 607
41. at Router->dispatch(*object*(Request)) in Kernel.php line 268
42. at Kernel->Illuminate\Foundation\Http\{closure}(*object*(Request)) in Pipeline.php line 53
43. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in CheckForMaintenanceMode.php line 46
44. at CheckForMaintenanceMode->handle(*object*(Request), *object*(Closure)) in Pipeline.php line 137
45. at Pipeline->Illuminate\Pipeline\{closure}(*object*(Request)) in Pipeline.php line 33
46. at Pipeline->Illuminate\Routing\{closure}(*object*(Request)) in Pipeline.php line 104
47. at Pipeline->then(*object*(Closure)) in Kernel.php line 150
48. at Kernel->sendRequestThroughRouter(*object*(Request)) in Kernel.php line 117
49. at Kernel->handle(*object*(Request)) i